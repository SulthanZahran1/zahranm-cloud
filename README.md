# zahranm-cloud

Central hub (zahranm.cloud) + WriteFreely blog (blog.zahranm.cloud) deployment.

The apex `zahranm.cloud` is the index of all public web properties: a hand-crafted
static HTML page served by nginx, replacing the retired Astro portfolio. The blog
is a single-user WriteFreely instance behind Traefik.

## Structure

```
hub/                  Static hub page (index.html) + nginx Dockerfile + compose
blog/                 WriteFreely instance (docker-compose.yml, data/)
docs/agents/          Wayfinder issue-tracker + domain docs
AGENTS.md             Agent conventions (issue tracking, domain docs)
CONTEXT.md            Domain glossary (implementation-free)
```

## Deploy

### Hub

```bash
cd hub
docker compose up -d --build
```

Serves `index.html` on port 80. Traefik routes `zahranm.cloud` to this container
(router `zahranm-cloud` in `~/hosted_projects/traefik/dynamic.yml`).

### Blog

```bash
cd blog
docker compose up -d
```

WriteFreely on `blog.zahranm.cloud`. Feed: `https://blog.zahranm.cloud/feed/`
(consumed by the hub's "latest from the blog" section).

## Hub page details

- Dark emerald mono registry: project links, recruiter copilot iframe
  (`/recruit`, same-origin, auto-sized to content), GitHub contribution graph,
  blog teaser (RSS, 15-min localStorage cache).
- No build step: plain HTML + inline CSS/JS.

## Notes

- Traefik config lives in `~/hosted_projects/traefik/` (not this repo).
- Planning is charted as a wayfinder map in this repo's Issues. See `AGENTS.md`.
