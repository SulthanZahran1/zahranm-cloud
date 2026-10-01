---
title: Sulthan Zahran Ma'ruf, AI engineer
description: AI engineer in Indonesia building AI agents and the infrastructure they need in production: tool gateways, observability, and tests for agent-written code.
canonical_url: https://zahranm.cloud/
last_updated: 2026-10-01
---

# Sulthan Zahran Ma'ruf, AI engineer: I build what AI agents run on

Tool gateways, agent runtimes, and the traces and evals that keep them honest.

Indonesia (UTC+7) · Rust · TypeScript · Go · Python · [CV (PDF)](https://zahranm.cloud/cv.pdf)

## One agent turn, traced

The home page opens on one Bella turn drawn as a trace (timings and token counts are sample values). A WhatsApp message, "Move my 3pm with Andi to tomorrow, same time, and let him know.", flows through these spans:

1. sambungapi · whatsapp.inbound
2. bella · plan (model)
3. bella · calendar.findEvent (minRole allowlisted)
4. bella · role gate on calendar.update (minRole member)
5. google · calendar.update
6. bella · reply (model)
7. sambungapi · whatsapp.send
8. console · review → dataset

A toggle switches who is asking. As a member, the gate passes and the event moves. As an allowlisted guest, the gate refuses calendar.update, Google is never contacted, and Bella replies that moving events needs a member account. Scrolling (or clicking a span) follows the turn through three chapters.

### 1. The gateway the message came through (sambungapi)

sambungapi is the Rust gateway I'm primary engineer on at Metatech. Replica of private code, sample data.

- Inbound webhooks (e.g. WhatsApp message.received) are committed to a durable outbox in the same transaction and delivered at-least-once by a worker that retries with the same Idempotency-Key.
- Tool calls don't go through the outbox: each one is idempotency-keyed, unseals the AES-256-GCM OAuth token for that call only, and writes an execution_logs row. Tokens are refreshed before expiry, or once on a 401.
- Demo: click one of seven providers to send a tool call, or try "token about to expire" and "incoming WhatsApp message".
- Stack: Rust, axum, PostgreSQL, OAuth 2.0, AES-256-GCM, outbox.

### 2. Same request, different person (Bella's role gate)

Bella is the AI chief of staff I'm a core engineer on: WhatsApp, Lark, Teams and web, one agent worker behind them. Real tool names and roles, sample request.

- Every tool declares a minimum role (admin, member, allowlisted), and a gate checks it before the call runs. A denial goes back to the model as `{ "ok": false, "verb": …, "error": { "code": "forbidden" } }`, and the model tells the user.
- Demo: "Book the sales sync for 10:00 tomorrow, and remind the team every Monday." asked as admin, member or allowlisted; calendar.freebusy, calendar.create and workflow.create are allowed or forbidden, and the reply changes to match.
- Stack: TypeScript, Hono, Go, MongoDB, Redis, LLM tool use.

### 3. Every turn, traced and reviewed (the observability console)

I built most of Bella's observability console: the waterfall, per-span tokens and cost, and a review loop where each verdict becomes a case in an eval dataset. Real span attributes and dataset names, sample values.

- Demo: inspect any span of the turn (token usage for model spans, minRole for tool calls, the gate decision, the outbox path for sambungapi spans), then review it: correct → confirmed-routing, wrong skill → hard-routing, bad reply → hard-quality.

## Same problems, solo

Open source and personal builds: trusting the code agents write, retrieval that weighs evidence, and real-time voice.

### Would your tests catch a bug? (rust_mutant, dart-mutant, gopher_mutant)

On crates.io. Plant small bugs; good tests fail. The playground is real rust-mutant 1.0.1 output (`--format json`) for a `fare(age, base)` function with two tests (adult, child).

- Before the missing test: MSI 80%. LOR `||` → `&&` survives, and the IR check (TCE) confirms it is real: the IR differs. After adding a toddler test: MSI 100%.
- AOI `/` → `/ 1 /` survives the tests but is marked `equivalent`: at opt-level=2 it compiles to the same IR hash, so it is excluded from the score. TCE runs only on survivors.
- 4 mutants (COR, LCR, RVR, AOD) don't compile and are excluded.
- Links: [rust_mutant](https://github.com/SulthanZahran1/rust_mutant), [crates.io](https://crates.io/crates/rust-mutant), [dart-mutant (Homebrew)](https://github.com/SulthanZahran1/dart-mutant), [gopher_mutant](https://github.com/SulthanZahran1/gopher_mutant), [how the IR check works](https://github.com/SulthanZahran1/rust_mutant/blob/main/crates/rust-mutant-tce/src/lib.rs).

### Search that weighs real skills (cv-search)

Retrieval, on request.

- Demo: the query "rust backend oauth integrations" over illustrative candidates, stepped through lexical + skill-field scoring (+8 per query term found anywhere, +18 when a term is a skill Gemini extracted into the structured profile), then an LLM rerank that rescores the lexical hits 0–100 with a reason.
- Stack: Go, React, Gemini extraction, LLM rerank.
- Links: [the scoring function](https://github.com/SulthanZahran1/cv-search-prototype/blob/main/api/main.go#L1228), [source](https://github.com/SulthanZahran1/cv-search-prototype).

### English → Korean, at the right speech level (honjang)

Voice, live.

- Demo: pick a listener (a shopkeeper or your manager) and the same English sentence is rendered in 해요체 (polite, everyday) or 합쇼체 (formal); an auto mode picks between the two.
- Pipeline: it waits for Deepgram's UtteranceEnd (1.2 s of silence), translates the whole reply, then speaks it clause by clause. Next (ADR-0004): stream LLM tokens into TTS.
- Stack: Expo, FastAPI, WebSocket, Deepgram Nova-3, OpenRouter, ElevenLabs.
- Links: [live app](https://honjang.zahranm.cloud), [source](https://github.com/SulthanZahran1/honjang), [ADR-0004](https://github.com/SulthanZahran1/honjang/blob/main/docs/adr/0004-hybrid-llm-tts-streaming.md).

## And software that moves atoms

LG Sinarmas, 2024 to now, EV-battery plant. MES, real-time dispatching and AMHS integration for a plant that runs around the clock, and a log analyzer that replays equipment handshakes as timing diagrams.

- Demo (illustrative incident): an AGV is stuck at EQ12. The analyzer replays the SEMI E84 handshake log (L_REQ and READY from the equipment, BUSY and COMPT from the AGV) as a timing diagram and diagnoses a TA3 timeout: READY took 94 s to drop after COMPT, so AGV07 was held at EQ12.
- Stack: MES, RTD, AMHS, C# / .NET, SQL Server, Python, KO–EN RAG.

## Career, as a timing diagram

Each lane is a track, each box a role; clicking one shows a one-line readout and the details. Lanes, top to bottom: AGENTS (Metatech), BUILDS (personal AI builds), FACTORY (LG Sinarmas, including the Korea deployment), LAB (FAAN Lab), FIELD (PLC internship, instrumentation engineer), EDU (B.Sc. Physics). Full history in the [CV (PDF)](https://zahranm.cloud/cv.pdf).

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

## Your visit, traced

Every demo a visitor touches emits a span, and the page draws the visit as a waterfall, like a Bella turn, with a playful "review this visit" step. The trace is kept in the tab's memory only; nothing is stored or sent anywhere.

## Contact

Building agents that touch real systems?

- Email: zsulthan9@gmail.com
- CV (PDF): https://zahranm.cloud/cv.pdf
- GitHub: https://github.com/SulthanZahran1
- LinkedIn: https://www.linkedin.com/in/sulthan-zahran-ui

Recruiter? [Paste a job description](https://zahranm.cloud/recruit/) and get a short, evidence-based pitch from my work profile ([source](https://github.com/SulthanZahran1/jd-pitcher)).

## Index: everything running on zahranm.cloud

Self-hosted on one VPS behind Traefik (a collapsed list at the bottom of the home page). Gated demos are available on request.

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

## Sitemap

- [Home](https://zahranm.cloud/)
- [About](https://zahranm.cloud/about)
- [Contact](https://zahranm.cloud/contact)
- [Privacy](https://zahranm.cloud/privacy)
- [Recruiter copilot](https://zahranm.cloud/recruit/)
- [llms.txt](https://zahranm.cloud/llms.txt)
- [XML sitemap](https://zahranm.cloud/sitemap.xml)
