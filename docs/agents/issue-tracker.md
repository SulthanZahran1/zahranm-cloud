# Issue tracker

- **Tracker:** GitHub Issues on this repo (`SulthanZahran1/zahranm-cloud`)
- **Tooling:** `gh` CLI (authenticated as SulthanZahran1)
- **Map:** one issue labelled `wayfinder:map` — the canonical chart. Its body holds Destination / Notes / Decisions so far / Not yet specified / Out of scope. It is an index, not a store: each decision lives in its ticket, the map gists it and links.
- **Tickets:** child issues of the map (linked via GitHub sub-issues API), each labelled `wayfinder:<type>`. Bodies start with `Part of #<map>`. Blocking is wired via GitHub's native `blocked_by` dependency API so the frontier renders visually.
- **Frontier:** open, unblocked, unassigned child tickets. A session claims a ticket by assigning it to itself before any work.
- **PRs as a request surface:** OFF (this repo tracks its own work; external PRs are not routed into the triage queue).
