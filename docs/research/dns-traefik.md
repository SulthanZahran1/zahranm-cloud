# Research: DNS + Traefik path for blog.zahranm.cloud

> Feeds wayfinder ticket #3 (cutover for the WriteFreely blog). Research only — no DNS
> mutations (Hostinger API/MCP) and no Traefik edits were performed. All facts below were
> verified live on 2026-08-04 unless marked as an assumption.

## 1. Current DNS state (verified)

```console
$ dig +short blog.zahranm.cloud A
(empty — no A record exists)

$ dig +short randomxyz-abc123.zahranm.cloud A
(empty — NO wildcard A record)

$ dig +short NS zahranm.cloud
ns1.dns-parking.com.
ns2.dns-parking.com.

$ dig +short honjang.zahranm.cloud A
72.61.213.90

$ dig +short zahranm.cloud A
72.61.213.90

$ dig zahranm.cloud SOA +noall +answer
zahranm.cloud.  564  IN  SOA  ns1.dns-parking.com. dns.hostinger.com. 2026073001 10000 2400 604800 600
```

- **Zone is served by Hostinger** (`ns1/ns2.dns-parking.com`, SOA contact `dns.hostinger.com`) —
  DNS changes must be made via Hostinger hPanel / Hostinger API, not Cloudflare.
- **No `blog` A record and no wildcard** — the record must be created.
- Existing subdomains (honjang, apex) resolve to the VPS `72.61.213.90`, confirming the target
  IP pattern. Observed TTLs (apex 3354s remaining) are consistent with a 3600s (1h) TTL;
  zone minimum is 600s.

## 2. DNS record to create (Hostinger)

| Field | Value |
| --- | --- |
| Type | `A` |
| Name | `blog` (→ `blog.zahranm.cloud`) |
| Value | `72.61.213.90` |
| TTL | `3600` (Hostinger default; matches existing records. Use `600` if a faster cutover/rollback is wanted) |

This is the **only** DNS change needed — the VPS already terminates TLS for other
`*.zahranm.cloud` subdomains via Traefik + Let's Encrypt, so no CNAME/other records required.

## 3. Traefik additions (`/home/dev/hosted_projects/traefik/dynamic.yml`)

Traefik v3.3, file-based dynamic config (mounted read-only into the container). The block below
mirrors the existing `honjang` router + service pattern exactly (certResolver `letsencrypt`,
`sec-headers` middleware, Docker bridge URL with trailing dot).

### Router (add under `http.routers`)

```yaml
    # ── blog.zahranm.cloud → WriteFreely blog ──
    blog:
      rule: "Host(`blog.zahranm.cloud`)"
      service: blog-writefreely
      tls:
        certResolver: letsencrypt
      middlewares:
        - sec-headers
```

### Service (add under `http.services`)

```yaml
    # ── blog.zahranm.cloud → WriteFreely (Docker bridge) ──
    blog-writefreely:
      loadBalancer:
        servers:
          - url: "http://blog-writefreely.:8080"
        passHostHeader: true
```

**Assumptions (verify at cutover):**

- Compose service name `blog-writefreely`, container name `writefreely`, app port `8080`.
- The trailing dot in `blog-writefreely.` follows the repo's bridge-network convention
  (bypasses Tailscale DNS — see existing services like `honjang-backend.`, `aquaflow.`).
- **No `healthCheck` block is included on purpose**: WriteFreely has **no `/health` endpoint**
  (verified in `writefreely/writefreely` `routes.go` — routes are `/`, `/api/*`,
  `/.well-known/*`). Honjang's `/health` check must NOT be copied blindly. If a health check is
  desired, point it at `/` (returns 200) — or omit it.

### Pitfall (known)

After editing `dynamic.yml`, restart Traefik — the file watcher does not re-evaluate all
changes:

```console
docker restart traefik
```

## 4. WriteFreely behind a reverse proxy

From official docs (writefreely.org/start → "Behind a reverse proxy", /docs/latest/admin/config)
and the WriteFreely source:

- **`config.ini` → `[app]` → `host` MUST be the public URL including scheme**:
  `host = https://blog.zahranm.cloud`. This is the single most common breakage behind a proxy —
  without it, generated links/buttons point at `localhost:port` (confirmed by
  discuss.write.as thread). Applies to both single-user and multi-user instances.
- **TLS stays off inside WriteFreely** (no `tls_cert_path`/`tls_key_path`, `autocert = false`) —
  Traefik terminates TLS. Keep `[server] port = 8080` (match the Traefik service URL).
- **`[server] bind` must be reachable from the Docker bridge** (e.g. `0.0.0.0`, not
  `localhost`) or Traefik's connection to the container IP will be refused.
- **Proxy must pass `Host` + `X-Forwarded-*`**: the official nginx example sets
  `Host`, `X-Real-IP`, `X-Forwarded-For`. Traefik's `passHostHeader: true` (included above) plus
  Traefik's default `X-Forwarded-For`/`X-Forwarded-Proto` headers satisfy this.
- **Federation endpoints must stay reachable**: `/.well-known/webfinger`,
  `/.well-known/nodeinfo`, `/.well-known/host-meta` (ActivityPub). Traefik's catch-all
  `Host()` rule passes every path through, so no extra config is needed — just don't add a
  `PathPrefix` to the router.
- **No websocket/upgrade requirements found** — WriteFreely is plain HTTP; no Traefik
  websocket headers or `http2` tweaks needed.

### Verify after deploy

1. `docker restart traefik` (per pitfall above), then check `traefik` logs for the new router
   and the Let's Encrypt cert issue for `blog.zahranm.cloud`.
2. `curl -sI https://blog.zahranm.cloud` → 200; confirm the TLS cert is the LE cert for
   `blog.zahranm.cloud` (SAN match).
3. `curl -s https://blog.zahranm.cloud/.well-known/webfinger?resource=acct:...` → federation
   endpoint responds (not 404/502).
4. Log in / publish a post and confirm rendered links use `https://blog.zahranm.cloud/...`
   (proves `[app] host` is correct).
5. Check the container health: `curl -s http://<container-ip>:8080/` from the host, and confirm
   `bind` is not localhost-only.

## 5. DNS propagation expectations (Hostinger)

- Hostinger support: propagation is automatic, A-record changes are typically fastest;
  **plan for up to 24h worst case, expect minutes to a few hours** (one Hostinger page says most
  updates ~30 min; docs.hostinger.com says 24–48h globally, often sooner).
- Verification plan for cutover: `dig +short blog.zahranm.cloud A @8.8.8.8` and
  `@1.1.1.1` (bypasses local resolver cache), plus dnschecker.org / whatsmydns.net for a global
  view. Since the VPS already serves other subdomains, a half-propagated record is harmless —
  only proceed with the Traefik router once the record resolves from the authoritative
  nameservers.
- TTL 3600 means cached negative results can linger up to an hour at recursive resolvers after
  creation; if cutover must be instant, create the record at TTL 600 (still fine for a blog).

## Sources

- WriteFreely "Getting Started" (reverse-proxy section + nginx example): https://writefreely.org/start
- WriteFreely config reference (`[app] host`, `[server] bind/port/tls`): https://writefreely.org/docs/latest/admin/config
- WriteFreely routes (federation endpoints, no `/health`): https://github.com/writefreely/writefreely/blob/main/routes.go
- Discuss thread — `host` value must be public URL behind proxy: https://discuss.write.as/t/writefreely-behind-a-nginx-reverse-proxy-buttons-dont-work-as-expected/20064
- Hostinger "What Is DNS Propagation": https://www.hostinger.com/support/4146975-what-is-dns-propagation-at-hostinger/
- Hostinger DNS docs: https://docs.hostinger.com/domains/dns
- Hostinger "How to Manage DNS Records": https://www.hostinger.com/support/1583249-how-to-manage-dns-records-at-hostinger/
- Local: `/home/dev/hosted_projects/traefik/dynamic.yml`, `/home/dev/hosted_projects/traefik/docker-compose.yml`
