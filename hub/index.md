---
title: Sulthan Zahran Ma'ruf, AI engineer
description: AI engineer in Indonesia building AI agents and the infrastructure they need in production: tool gateways, observability, and tests for agent-written code.
canonical_url: https://zahranm.cloud/
last_updated: 2026-09-30
---

# Sulthan Zahran Ma'ruf, AI engineer: I build AI agents that do real work

Agents that act on real accounts, the gateway they act through, and the console that shows what they did.

The home page opens with a simulated agent console, modelled on the observability console built for Bella. A visitor picks a request (for example "Move my 3pm"), watches the execution trace of model and tool-via-gateway spans with latency, tool calls, tokens and cost, reads the agent's reply, and marks the turn correct or wrong, as a human reviewer would.

## Selected work

Each tile on the home page is a small working model of something I built. Employer systems are simulated; personal projects link to source.

### The gateway agents act through (sambungapi)

Metatech, primary engineer. Private.

Agents call one API; the gateway holds every OAuth token encrypted, unlocks it only for the call, and talks to the provider. Built in-house to replace a third-party vendor.

- Demo: a diagram of agent → gateway (encrypted vault) → seven providers; clicking a provider animates a tool call and prints its log.
- Stack: Rust, axum, PostgreSQL, OAuth 2.0, durable outbox, multi-tenant.

### An AI chief of staff for a whole company (Bella)

Metatech, core engineer. Private.

Staff message it on WhatsApp, web or Lark; it handles email, meetings and cross-department updates within each person's permissions. I work on the engine that runs every turn and built the monitoring behind it.

- Demo: the request flow (WhatsApp, Lark or web → worker plans and calls tools → gateway to Gmail, Calendar, Lark → console with cost, trace and human review). The console at the top of the page simulates it.
- Pluggable engines: swap the reasoning backend without touching channels or tools.
- Per-role scope: what the agent may do follows the permissions of whoever asked.

### Would your tests catch a bug? Break the code and see (rust_mutant, dart-mutant, gopher_mutant)

Open source. rust_mutant is on crates.io.

Coding agents write a lot of tests. Mutation testing plants small bugs ("mutants") and checks the tests notice.

- Demo: a small Rust function with two tests; plant mutants one at a time or run them all, watch the mutation score, then add the missing boundary test and see the surviving mutants get killed.
- Links: [rust_mutant](https://github.com/SulthanZahran1/rust_mutant), [crates.io](https://crates.io/crates/rust-mutant), [dart-mutant (Homebrew)](https://github.com/SulthanZahran1/dart-mutant), [gopher_mutant](https://github.com/SulthanZahran1/gopher_mutant).

### Real-time English → Korean that knows who you're talking to (honjang)

Personal, voice. Live.

- Demo: pick a listener (a friend, a shopkeeper, your boss) and the same English sentence is rendered in Korean at the matching politeness level; "stream it" shows hear, think and speak lanes overlapping so audio starts mid-sentence (~300 ms budget).
- Links: [live app](https://honjang.zahranm.cloud), [source](https://github.com/SulthanZahran1/honjang).

### Software that keeps battery lines moving, 24/7 (LG Sinarmas)

EV-battery plant, 2024 to now. Industrial.

- Demo: an animated MES → RTD → AMHS loop, with vehicles moving between stocker, process and conveyor stations under RTD dispatch.
- Dispatch logic: RTD rules deciding which vehicle moves which material, where.
- Log → timing diagram: a Python + SQL analyzer that replays millions of AMHS events as signal timing.
- Equipment protocols: SECS/GEM and PLC handshakes between tools and the MES.
- KO–EN RAG: cross-language retrieval over Korean and English factory docs.
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
- Languages and infrastructure: Rust, Go, TypeScript, Python, C#, SQL, PostgreSQL, MongoDB, Redis, Docker, Traefik.

## Recruiter copilot

Paste a job description, get a pitch. The copilot at https://zahranm.cloud/recruit reads your role and writes a short, specific case for why my background fits. It's instructed to cite only evidence from my work profile. A small Go service with rate limits. Submitted descriptions and generated pitches are logged; IPs are stored only as hashes ([source](https://github.com/SulthanZahran1/jd-pitcher)).

## Index: everything running on zahranm.cloud

All self-hosted on one VPS behind Traefik. Gated demos are available on request.

| Property | Description | Status | Source |
| --- | --- | --- | --- |
| [honjang](https://honjang.zahranm.cloud) | English–Korean real-time voice translator with honorifics | live | [github](https://github.com/SulthanZahran1/honjang) |
| [recruit](https://zahranm.cloud/recruit) | Recruiter pitch generator: job description in, pitch out | live | [github](https://github.com/SulthanZahran1/jd-pitcher) |
| [relay](https://relay.zahranm.cloud) | AI-native CRM on an event-driven workflow engine | live | |
| [teach](https://teach.zahranm.cloud) | Harness Engineering: lessons on building an agent harness, modelled on Nous Research's Hermes | live | |
| [wiki](https://wiki.zahranm.cloud) | LLM-compiled wiki from 556 public LG Energy Solution blog posts, with chat and a graph | access token | [github](https://github.com/SulthanZahran1/lgensol-wiki) |
| [cv-search](https://cv-search.zahranm.cloud) | Résumé search: Gemini extraction, hybrid retrieval, LLM rerank | access token | [github](https://github.com/SulthanZahran1/cv-search-prototype) |
| [emrisearch](https://emrisearch.zahranm.cloud) | GUI for inspecting extreme-mass-ratio-inspiral search runs without Python | live | [github](https://github.com/SulthanZahran1/emrisearch-gui) |
| [aquaflow](https://aquaflow.zahranm.cloud) | Prepaid smart water metering for residents, managers and operators | live demo | |
| [dcim](https://dcim.zahranm.cloud) | Data-center infrastructure management on NetBox with a React front end | login | |
| [hkbp](https://hkbp.zahranm.cloud) | Church admin system moved from Laravel to Vue and Go, with self-hosted OIDC | members | [github](https://github.com/SulthanZahran1/hkbp-jatinegara) |
| [blog](https://blog.zahranm.cloud) | Writing | soon | [github](https://github.com/SulthanZahran1/zahranm-cloud) |

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
