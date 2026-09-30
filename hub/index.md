---
title: Sulthan Zahran Ma'ruf, AI engineer
description: AI engineer in Indonesia building AI agents and the infrastructure they need in production: tool gateways, observability, and tests for agent-written code.
canonical_url: https://zahranm.cloud/
last_updated: 2026-09-30
---

# Sulthan Zahran Ma'ruf, AI engineer: I build AI agents that do real work

I'm an AI engineer. At Metatech I build agents and the infrastructure they need in production: a tool gateway that lets them act on real accounts, observability that shows what each turn did and what it cost, and mutation testing for the tests they write. Before agents, I shipped smart-factory software for EV-battery lines at LG Sinarmas, so I build as if downtime is expensive.

- 8 weeks from empty repo to an in-house Rust tool gateway that lets agents act on real accounts.
- 2,000+ commits to Bella, an AI chief of staff: the agent worker and its observability console.
- 3 mutation testers for Rust, Go and Dart, built to check the tests agents write.
- 4.1M log entries analysed in 104 s by my incident tool at LG. Diagnosis went from hours or days to minutes.

## Selected work

Employer work is described, not linked. Personal projects link to source.

### sambungapi: an in-house Rust gateway that lets AI agents act on real accounts

Metatech, 2026. Primary engineer. Private (employer).

Our agents reach Gmail, calendars and chat apps through OAuth-connected tools, and that layer was rented from a vendor. I rebuilt it in-house as a single static Rust binary with its own /v1 API, hosted OAuth connect pages, and a step-by-step migration runbook for the agent platform. OAuth tokens are encrypted in our own Postgres instead of a vendor's.

- 8 weeks from empty repo to a production gateway: 3,100+ commits, 326 test files.
- AES-256-GCM at rest for every stored token, with multi-org tenancy and CSRF protection.
- Durable outbox drained by a separate worker mode, so events survive restarts.
- Stack: Rust, axum, PostgreSQL, OAuth 2.0, Google & Microsoft Graph APIs.

### Bella: an AI chief of staff across WhatsApp, Gmail, Calendar and Lark, and the observability console behind it

Metatech, 2026. Core engineer, team of several. Private (employer).

Bella takes requests in the channels a company already uses and carries them out with pluggable reasoning engines. My work centres on the worker that executes agent turns. I also built most of the observability console: each turn's cost, an execution waterfall, and a human-review loop that turns real conversations into labelled ground-truth datasets.

- 2,000+ commits of mine, mostly in the agent worker.
- 120+ ADRs written or revised, out of the team's ~340.
- Review → dataset: human review turns production conversations into ground truth.
- Stack: TypeScript, Hono, React, Go, MongoDB, Redis, LLM tool-use.

### *_mutant: mutation testers for Rust, Go and Dart, for checking the tests agents write

Personal, 2026. Author. Open source.

Mutation testing tells you whether your tests would catch a real bug, but it's slow and noisy. rust_mutant and dart-mutant run only the tests that cover a mutated line. rust_mutant also compares LLVM IR to throw out mutants that compile to identical code. Coding agents write a lot of tests now, and this is how I check whether those tests are any good.

- rust_mutant 1.0.1 on crates.io, with checksummed installers: [github](https://github.com/SulthanZahran1/rust_mutant), [crates.io](https://crates.io/crates/rust-mutant)
- dart-mutant: 17 operators, Homebrew tap, prebuilt binaries for 4 platforms: [github](https://github.com/SulthanZahran1/dart-mutant)
- gopher_mutant: 21 operator classes for Go: [github](https://github.com/SulthanZahran1/gopher_mutant)
- Stack: Rust, LLVM IR, coverage instrumentation.

### Smart-factory AMHS: keeping EV-battery lines moving (MES, dispatching and material handling)

LG Sinarmas, 2024 to now. Software engineer. Industrial.

I build and integrate the software between the manufacturing execution system, real-time dispatching and the vehicles and conveyors that move material between processes. I also bridge Indonesian factory operations and the Korean engineering team. When a stall needs diagnosing, I use the Timing Diagram Analyzer I wrote: it turns millions of material-handling log entries into a timing view, which is also why this site's timeline is a timing diagram.

- 4,120,953 entries analysed in 104 s. Diagnosis went from hours or days to minutes, and the whole team uses it.
- 1 of 4 engineers selected for a two-month deployment to LG Energy Solution Ochang, Korea.
- 15+ integration issues across AGVs, conveyors and stockers, fixed before production.
- KO–EN RAG: cross-language retrieval over Korean and English factory documentation.
- Stack: C# / .NET, WPF, React, Python, SQL Server, Oracle, SECS/GEM, LangChain.

### honjang: a real-time English–Korean voice translator that tracks honorifics

Personal, 2026. Author. Open source: [github](https://github.com/SulthanZahran1/honjang)

Korean speech changes with who you're talking to, and literal translation misses that. honjang streams speech through recognition, an LLM and speech synthesis, with barge-in, a walkie-talkie mode and an always-on agent mode. LLM output streams to speech in chunks split at Korean clause boundaries, so audio can start before the sentence is done.

- ~300 ms design budget to first audio with clause-level streaming.
- < Rp2,000/min cost ceiling, with speech, LLM and voice each costed per minute.
- Stack: Expo, FastAPI, WebSocket, Deepgram Nova-3, OpenRouter, ElevenLabs.

## Experience: 2019 to now, drawn as a timing diagram

On the home page the career is drawn as a timing diagram. Lanes, top to bottom: AGENTS (Metatech), FACTORY (LG Sinarmas, including the Korea deployment), LAB (FAAN Lab), FIELD (PLC internship, instrumentation engineer), EDU (B.Sc. Physics).

- **Apr 2026 to now: Software Engineer, Metatech** (part-time, remote). AI agents, the observability tools that watch them, and the messaging infrastructure they run on.
- **Apr 2024 to now: Software Engineer, LG Sinarmas Technology Solutions** (Karawang). Smart-factory software (MES, real-time dispatching, AMHS) for an EV-battery plant.
  - Shipped 8+ internal applications; one saves 50 person-hours a day.
  - Designed the team's onboarding program and trained 15+ software engineers.
  - Built the Timing Diagram Analyzer (4.1M log entries in 104 s) and a Korean–English RAG pipeline over factory documentation.
  - Liaison between Indonesian factory operations and the Korean engineering HQ.
- **Aug to Oct 2025: AMHS Integration Specialist, LG Energy Solution Ochang** (South Korea). Selected as one of four engineers for a two-month deployment to integrate material handling before production. Fixed 15+ pre-production issues across AGVs, conveyors and stockers.
- **Jan to Mar 2024: Instrumentation Engineer, PT Polychemie Asia Pacific Permai.** Project-based instrumentation work.
- **Oct 2023 to Mar 2024: Software Engineering Research Assistant, FAAN Laboratory, Physics, Universitas Indonesia.** Built lab software that improved the synthesis–testing cycle by roughly 800%.
- **Aug 2019 to Jul 2023: B.Sc. Physics, Instrumentation, Universitas Indonesia.**
  - 4th place at IndySCC22, the only team from Southeast Asia.
  - Programming lead for the KRTMI robotics contest, UI Robotics Team.
  - PLC intern at PT Sugitama Intiarto, Jun to Jul 2022.

## How I work: I write code with agents, and build the checks that catch their mistakes

- **Decisions go on paper.** Every hard-to-reverse choice gets an ADR. On Bella the team keeps ~340, and I've written or revised 120+. Agents and new teammates both read the reasoning, not just the code.
- **Test the tests.** Agents write a lot of tests, so I measure whether those tests would catch a real bug. That's why I built mutation testers.
- **Measure before you trust.** On a factory floor, a bad deploy stops a production line. I carry that into software: build the observability first, and don't trust anything until it's measured.

Skills. AI / agents: tool-use & MCP, RAG, agent harnesses, LLM observability, voice pipelines. Languages: Rust, Go, TypeScript, Python, C#, SQL. Industrial: MES, AMHS, RTD, PLC, SECS/GEM, AGV integration. Infra: PostgreSQL, MongoDB, Redis, Docker, Traefik, Linux.

## Recruiter copilot

Paste a job description, get a pitch. The copilot at https://zahranm.cloud/recruit reads a role and writes a short, specific case for why my background fits. It's instructed to cite only evidence from my work profile. It's a small Go service with rate limits. Submitted descriptions and generated pitches are logged; IPs are stored only as hashes ([source](https://github.com/SulthanZahran1/jd-pitcher)).

## Index: everything running on zahranm.cloud

All self-hosted on one VPS behind Traefik. Gated demos are available on request.

| Property | Description | Status | Source |
| --- | --- | --- | --- |
| [zahranm.cloud/recruit](https://zahranm.cloud/recruit) | Recruiter pitch generator: job description in, evidence-backed pitch out | live | [github](https://github.com/SulthanZahran1/jd-pitcher) |
| [relay](https://relay.zahranm.cloud) | AI-native CRM on an event-driven workflow engine: sessions, tickets, web chat | live | |
| [teach](https://teach.zahranm.cloud) | Harness Engineering: a lesson series on building an agent harness, modelled on Nous Research's Hermes | live | |
| [aquaflow](https://aquaflow.zahranm.cloud) | Prepaid smart water metering with resident, building-manager and operator roles | live demo | |
| [emrisearch](https://emrisearch.zahranm.cloud) | GUI for the emrisearch pipeline: inspect extreme-mass-ratio-inspiral search runs without writing Python | live | [github](https://github.com/SulthanZahran1/emrisearch-gui) |
| [wiki](https://wiki.zahranm.cloud) | LLM-compiled wiki built from 556 public LG Energy Solution blog posts, with chat and a graph view | access token | [github](https://github.com/SulthanZahran1/lgensol-wiki) |
| [cv-search](https://cv-search.zahranm.cloud) | Résumé search: Gemini extraction with fallbacks, hybrid retrieval, optional LLM rerank | access token | [github](https://github.com/SulthanZahran1/cv-search-prototype) |
| [dcim](https://dcim.zahranm.cloud) | Data-center infrastructure management built on NetBox with a React front end | login | |
| [hkbp](https://hkbp.zahranm.cloud) | Church admin system moved from Laravel to Vue and Go, with a self-hosted OIDC login | members | [github](https://github.com/SulthanZahran1/hkbp-jatinegara) |
| [honjang](https://honjang.zahranm.cloud) | English–Korean real-time voice translator (see above) | live | [github](https://github.com/SulthanZahran1/honjang) |
| [blog](https://blog.zahranm.cloud) | Writing | soon | [github](https://github.com/SulthanZahran1/zahranm-cloud) |

## Contact

Building agents that touch real systems? Let's talk. Agents, the tools and infrastructure around them, or the evals that tell you whether they actually work. Tell me what you're working on and what's in the way.

- Email: zsulthan9@gmail.com
- GitHub: https://github.com/SulthanZahran1
- LinkedIn: https://www.linkedin.com/in/sulthan-zahran-ui

## Sitemap

- [Home](https://zahranm.cloud/)
- [About](https://zahranm.cloud/about)
- [Contact](https://zahranm.cloud/contact)
- [Privacy](https://zahranm.cloud/privacy)
- [Recruiter copilot](https://zahranm.cloud/recruit)
- [llms.txt](https://zahranm.cloud/llms.txt)
- [XML sitemap](https://zahranm.cloud/sitemap.xml)
