# WriteFreely Deployment Specifics

Research for issue #2 — how to run a single-user private WriteFreely blog on this VPS. All claims below were verified empirically (image pull + inspect, live test containers) and cross-checked against official docs.

## 1. Image: use community `algernon/writefreely`, not official

- Official `writeas/writefreely` is **stale**: last push 2021-06-30 (v0.12 era). Official docs still say there is no official Docker pathway to production.
- `nixcloud/writefreely` does **not exist** on Docker Hub.
- **Pinned reference:** `algernon/writefreely:0.16.0-1`
  - digest `sha256:4269b3bd04e75858721350a5191e7eb611007d8b3369c1b66b33fa39e197d3b9`, pushed 2025-10-21
  - image ~17.8 MB, Alpine, runs as UID 5000, VOLUME `/data`, exposes **8080/tcp**
  - has `wget` (no `curl`)
- **CVE caveat:** WriteFreely v0.17.0 (2026-07-17) fixed critical CVEs including "Prevent signups via /auth/signup with closed registrations" (affects <= 0.16). Options:
  - safest: self-build with build arg `WRITEFREELY_VERSION=v0.17.1` (documented on Docker Hub)
  - or deploy 0.16 with `single_user=true` + `open_registration=false` and accept the risk (single-user instance materially reduces exposure)

## 2. Single-user private mode

Entrypoint auto-writes `config.ini` on first boot from env vars:

```yaml
WRITEFREELY_SINGLE_USER=true
WRITEFREELY_OPEN_REGISTRATION=false
WRITEFREELY_HOST=https://blog.zahranm.cloud   # CRITICAL: feed links derive from this
WRITEFREELY_SITE_NAME=<name>
WRITEFREELY_ADMIN_USER=zahran
WRITEFREELY_ADMIN_PASSWORD=<pass>            # entrypoint auto-runs --create-admin
```

Manual admin creation (verified working):

```bash
docker exec -w /data <ctr> /writefreely/writefreely -c /data/config.ini db init
docker exec -w /data <ctr> /writefreely/writefreely -c /data/config.ini --create-admin "zahran:<pass>"
```

Pitfalls found live:
- **`admin` is a reserved username** (use e.g. `zahran`)
- `docker exec` defaults to workdir `/writefreely`, so always use `-w /data` or pass `-c /data/config.ini` — otherwise the CLI touches the wrong `writefreely.db`

## 3. Storage / backup

Everything lives in `/data`: `config.ini`, `writefreely.db` (SQLite, ~196 KB fresh), `keys/` (email.aes256). **Backup surface = the whole `/data` dir.** Entrypoint auto-runs `db migrate` on boot, keeping a timestamped `.db` backup if migration changed data.

## 4. Port + healthcheck

- Port: **8080**
- **There is no `/api/health`** (verified 404). Working healthcheck (verified 200): `/api/nodeinfo`
- `wget -q -O /dev/null http://localhost:8080/api/nodeinfo` (curl absent)
- `/` returns 404 until an admin exists, 200 after

## 5. RSS (hub teaser source)

- Single-user blog lives at instance root; feed = **`/feed/`** → `https://blog.zahranm.cloud/feed/` (verified 200, valid RSS 2.0)
- Feed `<link>`/post URLs come from `[app] host` in config.ini — verified live: `host = https://blog.zahranm.cloud` rewrites feed links to the custom domain

## 6. Footprint (measured)

- Container RSS: ~13.6-13.8 MiB idle — no swap-thrash concern
- Image 17.8 MB; data dir ~204 KB fresh

## Sources

- hub.docker.com/r/algernon/writefreely
- writefreely.org/docs/main/admin/config, /commands, /docker
- github.com/writefreely/writefreely/releases/tag/v0.17.0, v0.17.1
- writefreely.org/docs/main/writer/following
