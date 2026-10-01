---
title: Sulthan Zahran Ma'ruf, AI engineer
description: AI engineer in Indonesia building AI agents and the infrastructure they need in production: tool gateways, observability, and tests for agent-written code.
canonical_url: https://zahranm.cloud/
last_updated: 2026-10-01
---

# Sulthan Zahran Ma'ruf, AI engineer: I build AI agents that do real work

Sulthan Zahran Ma'ruf · AI engineer.

Right now: a Rust OAuth gateway for agents, and the console that traces every turn.

At a glance: Indonesia (UTC+7) · Rust, TypeScript, Go, Python · Now: AI agents at Metatech (part-time, remote) alongside smart-factory software at LG Sinarmas (full-time). [CV (PDF)](https://zahranm.cloud/cv.pdf).

The home page opens with an agent console that replays a real turn shape with sample data, modelled on the observability console built for Bella. A visitor picks a request ("Move my 3pm" on WhatsApp, "What needs me today?" on Lark, "Brief me for tomorrow" on web) and watches the execution trace of model and tool spans (calendar.findEvent, calendar.freebusy, calendar.update, gmail.unread, calendar.upcoming, gmail.search ×4, lark.readChat) with latency, tool calls, tokens and cost. Clicking a span opens an inspector with Bella's span attributes (token usage for model spans; gates, idempotency key and provider API for tool spans). Reviewing the turn turns it into a dataset case: correct → confirmed-routing, wrong skill → hard-routing, bad reply → hard-quality.

## Selected work

Don't read about it. Play with it. Employer code is private, so those tiles replay the real design with sample data; personal projects link to source.

### The gateway agents act through (sambungapi)

Metatech, primary engineer. Private code, replica.

One API for agents. OAuth tokens stay sealed until the call; inbound webhooks go through a durable outbox.

- Tool call: claims an idempotency key, decrypts the provider's AES-256-GCM token for that call only, calls the provider and writes an execution_logs row. Tool calls do not go through the outbox.
- Token refresh: a token close to expiry is refreshed before the call (or once on a 401) and re-encrypted with a fresh nonce.
- Inbound webhook (e.g. WhatsApp message.received): written to the outbox in the same transaction, then a worker delivers it at-least-once, retrying with the same Idempotency-Key.
- Demo: agent → gateway (encrypted token vault) → seven providers; click a provider, a token about to expire, or an incoming WhatsApp message.
- Stack: Rust, axum, PostgreSQL, OAuth 2.0, durable outbox, multi-tenant.

### An AI chief of staff for a whole company (Bella)

Metatech, core engineer. Private code, replica.

On WhatsApp, Lark, Teams and web. Every tool call passes a role gate before it runs.

- Roles: admin, member, allowlisted. Each tool declares a minRole, and the role gate compares it with the asker's role before every call.
- Demo: the same request ("Book the sales sync for 10:00 tomorrow, and remind the team every Monday.") asked as admin, member or allowlisted. Each call (calendar.freebusy, calendar.create, workflow.create) is allowed or forbidden; a denial goes back to the model as `{ "ok": false, "verb": …, "error": { "code": "forbidden" } }`, and the model tells the user.
- Stack: TypeScript, Hono, React, Go, MongoDB, Redis, LLM tool use.

### Would your tests catch a bug? Break the code and see (rust_mutant, dart-mutant, gopher_mutant)

Open source. rust_mutant is on crates.io.

Plant small bugs; good tests fail. The demo is real rust-mutant 1.0.1 output (`--format json`) for a `fare(age, base)` function with two tests (adult, child).

- Before the missing test: MSI 80%. LOR `||` → `&&` survives, and the IR check (TCE) confirms it is real: the IR differs. After adding a toddler test: MSI 100%.
- AOI `/` → `/ 1 /` survives the tests but is marked `equivalent`: at opt-level=2 it compiles to the same IR hash, so it is excluded from the score instead of counted as a gap. TCE runs only on survivors.
- 4 mutants (COR, LCR, RVR, AOD) don't compile and are excluded.
- Links: [rust_mutant](https://github.com/SulthanZahran1/rust_mutant), [crates.io](https://crates.io/crates/rust-mutant), [dart-mutant (Homebrew)](https://github.com/SulthanZahran1/dart-mutant), [gopher_mutant](https://github.com/SulthanZahran1/gopher_mutant), [how the IR check works](https://github.com/SulthanZahran1/rust_mutant/blob/main/crates/rust-mutant-tce/src/lib.rs).

### Real-time English → Korean that knows who you're talking to (honjang)

Personal, voice. Live.

- Demo: pick a listener (a shopkeeper or your manager) and the same English sentence is rendered in 해요체 (polite, everyday) or 합쇼체 (formal); an auto mode picks between the two.
- Pipeline: it waits for Deepgram's UtteranceEnd (1.2 s of silence), translates the whole reply, then speaks it clause by clause. Next (ADR-0004): stream LLM tokens into TTS.
- Stack: Expo, FastAPI, WebSocket, Deepgram Nova-3, OpenRouter, ElevenLabs.
- Links: [live app](https://honjang.zahranm.cloud), [source](https://github.com/SulthanZahran1/honjang), [ADR-0004](https://github.com/SulthanZahran1/honjang/blob/main/docs/adr/0004-hybrid-llm-tts-streaming.md).

### Search that weighs real skills, not keyword hits (cv-search)

Personal, retrieval. On request.

- Demo: the query "rust backend oauth integrations" over illustrative candidates, stepped through lexical + skill-field scoring (+8 per query term found anywhere, +18 when a term is a skill Gemini extracted into the structured profile), then an LLM rerank that rescores the lexical hits 0–100 with a reason. The candidate with direct evidence climbs to the top.
- Stack: Go, React, Gemini extraction, LLM rerank.
- Links: [source](https://github.com/SulthanZahran1/cv-search-prototype).

### Software that keeps battery lines moving, 24/7 (LG Sinarmas)

EV-battery production, 2024 to now. Private code, replica.

- Demo: an illustrative incident on an MES → RTD → AMHS loop: an AGV is stuck at EQ12. The analyzer replays the SEMI E84 handshake log (L_REQ and READY from the equipment, BUSY and COMPT from the AGV) as a timing diagram and diagnoses a TA3 timeout: READY took 94 s to drop after COMPT, so AGV07 was held at EQ12. The loop halts on the alarm.
- Stack: MES, RTD, AMHS, C# / .NET, SQL Server, Python, KO–EN RAG.

## Experience

On the home page the career is drawn as a clickable timing diagram. Lanes, top to bottom: AGENTS (Metatech), BUILDS (personal AI builds), FACTORY (LG Sinarmas, including the Korea deployment), LAB (FAAN Lab), FIELD (PLC internship, instrumentation engineer), EDU (B.Sc. Physics).

- **Software Engineer, AI agents.** Metatech, part-time, remote. Apr 2026 to now.
  - Built sambungapi in Rust (axum): an OAuth tool gateway with an AES-256-GCM token vault, multi-org tenancy and a durable outbox, through which agents act on Google, Microsoft, GitHub, Slack, Notion, Linear and Jira.
  - Core engineer on Bella's agent worker (TypeScript): the runtime that plans, calls tools (Gmail, Calendar, Lark and more) and replies on every turn, over WhatsApp, Lark, Teams and web.
  - Built the messaging engine between apps and chat platforms, and the observability behind it: per-turn cost, execution waterfalls, and a human-review loop that produces ground-truth datasets.
- **AI builds, shipped.** Personal, open source, self-hosted. Jun 2026 to now.
  - lgensol-wiki (Jun): an LLM-compiled wiki with chat and a knowledge graph over public LGES posts.
  - cv-search (Jun): résumé search with Gemini extraction, deterministic scoring and an LLM rerank.
  - honjang (Jul): real-time English → Korean voice translation that picks the right honorific level.
  - rust_mutant, dart-mutant, gopher_mutant (Aug): mutation testing for the tests coding agents write.
  - recruit (Aug): the pitch copilot below, grounded only in my work profile.
- **Software Engineer, smart factory.** LG Sinarmas Technology Solutions, Karawang. Apr 2024 to now.
  - MES, real-time dispatching (RTD) and AMHS integration for an EV-battery plant: C# / .NET, WPF, SQL Server and Oracle.
  - Wrote a Python + SQL log analyzer that replays AMHS events as timing diagrams for incident diagnosis.
  - Built a Korean–English RAG pipeline (LangChain, PostgreSQL) over factory documentation, and automated monitoring across the plant's IoT sensor web interfaces.
- **AMHS Integration Specialist.** LG Energy Solution Ochang, South Korea. Aug to Oct 2025.
  - Integration-tested AGV, conveyor and stocker subsystems against the MES before production, and fixed the integration bugs in code.
- **Instrumentation Engineer.** PT Polychemie Asia Pacific Permai. Jan to Mar 2024.
  - Prototyped an ADC-based monitoring rig so temperature and flow could be logged on a PC instead of by hand; evaluated DAQ, PLC and Arduino options and wrote the plotting GUI.
- **Research Assistant, software.** FAAN Laboratory, Physics, Universitas Indonesia. Oct 2023 to Mar 2024.
  - Built the lab's MATLAB spectroscopy app: real-time acquisition, peak detection and fitting, Savitzky–Golay filtering and Tauc plots.
- **PLC Intern.** PT Sugitama Intiarto. Jun to Jul 2022.
  - First contact with industrial control software.
- **B.Sc. Physics, Instrumentation.** Universitas Indonesia. Aug 2019 to Jul 2023.
  - IndySCC22 student cluster competition (HPC): 4th place, the only team from Southeast Asia.
  - Programming lead for the UI Robotics Team at KRTMI.

## Skills

- AI: AI agents, tool use & MCP, RAG, LLM observability, voice pipelines, evals, mutation testing.
- Languages: Rust, Go, TypeScript, Python.

## Recruiter copilot

Paste a job description, get a pitch. The copilot at https://zahranm.cloud/recruit is grounded only in my work profile: pick a sample role or paste your own. A small Go service with rate limits. Submitted descriptions and generated pitches are logged; IPs are stored only as hashes ([source](https://github.com/SulthanZahran1/jd-pitcher)).

## Index: everything running on zahranm.cloud

All self-hosted on one VPS behind Traefik. Gated demos are available on request.

| Property | Description | Status | Source |
| --- | --- | --- | --- |
| [honjang](https://honjang.zahranm.cloud) | English–Korean real-time voice translator with honorifics | live | [source](https://github.com/SulthanZahran1/honjang) |
| [recruit](https://zahranm.cloud/recruit) | LLM pitch generator: job description in, evidence-grounded pitch out | live | [source](https://github.com/SulthanZahran1/jd-pitcher) |
| [relay](https://relay.zahranm.cloud) | AI-native CRM on an event-driven workflow engine | live | |
| [teach](https://teach.zahranm.cloud) | Course: build an agent harness from scratch | live | |
| [wiki](https://wiki.zahranm.cloud) | LLM-compiled wiki with chat and a knowledge graph, from 556 public LGES posts | on request | [source](https://github.com/SulthanZahran1/lgensol-wiki) |
| [cv-search](https://cv-search.zahranm.cloud) | Résumé search: Gemini extraction, lexical + skill scoring, LLM rerank | on request | [source](https://github.com/SulthanZahran1/cv-search-prototype) |

Other builds:

| Property | Description | Status | Source |
| --- | --- | --- | --- |
| [emrisearch](https://emrisearch.zahranm.cloud) | GUI for extreme-mass-ratio-inspiral search runs | live | [source](https://github.com/SulthanZahran1/emrisearch-gui) |
| [aquaflow](https://aquaflow.zahranm.cloud) | Prepaid smart water metering, three roles | live | |
| [dcim](https://dcim.zahranm.cloud) | Data-center infrastructure management on NetBox + React | on request | |
| [hkbp](https://hkbp.zahranm.cloud) | Church admin system: Laravel → Vue + Go, self-hosted OIDC | on request | [source](https://github.com/SulthanZahran1/hkbp-jatinegara) |

## Contact

Building agents that touch real systems?

- Email: zsulthan9@gmail.com
- GitHub: https://github.com/SulthanZahran1
- LinkedIn: https://www.linkedin.com/in/sulthan-zahran-ui
- CV (PDF): https://zahranm.cloud/cv.pdf

## Sitemap

- [Home](https://zahranm.cloud/)
- [About](https://zahranm.cloud/about)
- [Contact](https://zahranm.cloud/contact)
- [Privacy](https://zahranm.cloud/privacy)
- [Recruiter copilot](https://zahranm.cloud/recruit)
- [llms.txt](https://zahranm.cloud/llms.txt)
- [XML sitemap](https://zahranm.cloud/sitemap.xml)
