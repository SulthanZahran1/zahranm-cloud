---
title: Sulthan Zahran Ma'ruf, AI engineer
description: AI engineer in Indonesia building AI agents and the infrastructure they need in production: tool gateways, observability, and tests for agent-written code.
canonical_url: https://zahranm.cloud/
last_updated: 2026-10-01
---

# Sulthan Zahran Ma'ruf, AI engineer: I build AI agents that do real work

Sulthan Zahran Ma'ruf · AI engineer at Metatech · Indonesia.

Right now: a Rust OAuth gateway that agents act through, the runtime that executes each turn, and the console that traces it.

The home page opens with a simulated agent console, modelled on the observability console built for Bella. A visitor picks a request ("Move my 3pm" on WhatsApp, "What needs me today?" on Lark, "Brief me for tomorrow" on web), watches the execution trace of model and tool spans with latency, tool calls, tokens and cost, clicks a tool span to follow the call through the gateway, reads the agent's reply, and marks the turn correct or wrong, as a human reviewer would.

## Selected work

Don't read about it. Play with it. Each tile on the home page is a small working model of something I built. Employer systems are simulated; personal projects link to source.

### The gateway agents act through (sambungapi)

Metatech, primary engineer. Private.

Agents call one API; the gateway holds every OAuth token encrypted, unlocks it only for the call, and talks to the provider. Built in-house to replace a third-party vendor.

- Demo: a diagram of agent → gateway (encrypted token vault) → seven providers; clicking a provider sends a call through the vault and prints its log.
- Stack: Rust, axum, PostgreSQL, OAuth 2.0, durable outbox, multi-tenant.

### An AI chief of staff for a whole company (Bella)

Metatech, core engineer. Private.

Staff message it on WhatsApp, Lark or web. It runs email, meetings and cross-team updates, and every tool call is checked against the asker's role. I work on the worker that runs each turn.

- Flow: chat → worker → gateway → console.
- Demo: the same request ("Book the whole sales team for 10:00 tomorrow.") asked as a director, staff member or intern. Each tool call (calendar free/busy, calendar create) is allowed or blocked by role, and the reply changes to match.
- Stack: TypeScript, Hono, React, Go, MongoDB, Redis, LLM tool use.

### Would your tests catch a bug? Break the code and see (rust_mutant, dart-mutant, gopher_mutant)

Open source. rust_mutant is on crates.io.

Coding agents write a lot of tests. Mutation testing plants small bugs ("mutants") and checks the tests notice. rust_mutant also compares LLVM IR to skip mutants that compile to identical code.

- Demo: a small Rust function with two tests; plant mutants one at a time or run them all, watch the mutation score, then add the missing boundary test and see the surviving mutants get killed.
- Links: [rust_mutant](https://github.com/SulthanZahran1/rust_mutant), [crates.io](https://crates.io/crates/rust-mutant), [dart-mutant (Homebrew)](https://github.com/SulthanZahran1/dart-mutant), [gopher_mutant](https://github.com/SulthanZahran1/gopher_mutant).

### Real-time English → Korean that knows who you're talking to (honjang)

Personal, voice. Live.

- Demo: pick a listener (a friend, a shopkeeper, your boss) and the same English sentence is rendered in Korean at the matching politeness level; hear, think and speak lanes overlap so first audio starts mid-sentence (≈300 ms design budget).
- Stack: Expo, FastAPI, WebSocket, Deepgram Nova-3, OpenRouter, ElevenLabs.
- Links: [live app](https://honjang.zahranm.cloud), [source](https://github.com/SulthanZahran1/honjang).

### Software that keeps battery lines moving, 24/7 (LG Sinarmas)

EV-battery production, 2024 to now. Industrial.

- Demo: an animated MES → RTD → AMHS loop; clicking a station (stocker, process, conveyor, dispatch) explains the software behind it.
- Log → timing diagram: an illustrative AMHS handshake log run through the analyzer, which redraws it as signal timing and flags the stuck signal (READY stayed high after COMPLETE, blocking the vehicle).
- Stack: MES, RTD, AMHS, C# / .NET, SQL Server, Python, KO–EN RAG.

## Experience

On the home page the career is drawn as a clickable timing diagram. Lanes, top to bottom: AGENTS (Metatech), FACTORY (LG Sinarmas, including the Korea deployment), LAB (FAAN Lab), FIELD (PLC internship, instrumentation engineer), EDU (B.Sc. Physics).

- **Software Engineer, AI agents.** Metatech, part-time, remote. Apr 2026 to now.
  - Built sambungapi in Rust (axum): an OAuth tool gateway with an AES-256-GCM token vault, multi-org tenancy and a durable outbox, through which agents act on Google, Microsoft, GitHub, Slack, Notion, Linear and Jira.
  - Core engineer on Bella's agent worker (TypeScript): the runtime that plans, calls tools and replies on every turn, across WhatsApp, Gmail, Calendar and Lark.
  - Built the messaging engine between apps and chat platforms, and the observability behind it: per-turn cost, execution waterfalls, and a human-review loop that produces ground-truth datasets.
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

- AI: AI agents, tool use & MCP, RAG, LLM observability, voice pipelines, evals & mutation testing.
- Languages: Rust, Go, TypeScript, Python.

## Recruiter copilot

Paste a job description, get a pitch. The copilot at https://zahranm.cloud/recruit reads your role and writes a short, specific case for why my background fits. It's instructed to cite only evidence from my work profile. A small Go service with rate limits. Submitted descriptions and generated pitches are logged; IPs are stored only as hashes ([source](https://github.com/SulthanZahran1/jd-pitcher)).

## Index: everything running on zahranm.cloud

All self-hosted on one VPS behind Traefik. Gated demos are available on request.

| Property | Description | Status | Source |
| --- | --- | --- | --- |
| [honjang](https://honjang.zahranm.cloud) | English–Korean real-time voice translator with honorifics | live | [source](https://github.com/SulthanZahran1/honjang) |
| [recruit](https://zahranm.cloud/recruit) | LLM pitch generator: job description in, evidence-grounded pitch out | live | [source](https://github.com/SulthanZahran1/jd-pitcher) |
| [relay](https://relay.zahranm.cloud) | AI-native CRM on an event-driven workflow engine | live | |
| [teach](https://teach.zahranm.cloud) | Harness Engineering: building an agent harness, modelled on Hermes | live | |
| [wiki](https://wiki.zahranm.cloud) | LLM-compiled wiki with chat and a knowledge graph, from 556 public LGES posts | on request | [source](https://github.com/SulthanZahran1/lgensol-wiki) |
| [cv-search](https://cv-search.zahranm.cloud) | Résumé search: Gemini extraction, hybrid retrieval, LLM rerank | on request | [source](https://github.com/SulthanZahran1/cv-search-prototype) |

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

## Sitemap

- [Home](https://zahranm.cloud/)
- [About](https://zahranm.cloud/about)
- [Contact](https://zahranm.cloud/contact)
- [Privacy](https://zahranm.cloud/privacy)
- [Recruiter copilot](https://zahranm.cloud/recruit)
- [llms.txt](https://zahranm.cloud/llms.txt)
- [XML sitemap](https://zahranm.cloud/sitemap.xml)
