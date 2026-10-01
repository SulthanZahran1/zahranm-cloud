---
title: About Sulthan Zahran Ma'ruf, AI engineer
description: About Sulthan Zahran Ma'ruf, an AI engineer in Indonesia building agents and the infrastructure they need in production.
canonical_url: https://zahranm.cloud/about
last_updated: 2026-10-01
---

# About Sulthan Zahran Ma'ruf, AI engineer

Sulthan Zahran Ma'ruf is an AI engineer in Indonesia. He builds AI agents that do real work, and the infrastructure they need in production: the gateway agents act through, the engine that runs each turn, and the monitoring that shows what they did. Now: AI agents at Metatech (part-time, remote), alongside smart-factory software at LG Sinarmas (full-time). Based in Indonesia (UTC+7). [CV (PDF)](https://zahranm.cloud/cv.pdf).

## Agents and infrastructure

**Software Engineer, AI agents. Metatech, part-time, remote, Apr 2026 to now.**

- sambungapi: a Rust (axum) OAuth tool gateway with an AES-256-GCM token vault, multi-org tenancy and a durable outbox, through which agents act on Google, Microsoft, GitHub, Slack, Notion, Linear and Jira. Agents call one API; each tool call unseals the provider token for that call only, is idempotency-keyed and writes an execution_logs row, and tokens are refreshed before expiry or once on a 401. The durable outbox carries inbound webhooks (such as a WhatsApp message.received): written in the same transaction, then delivered at-least-once by a worker that retries with the same Idempotency-Key. Built in-house to replace a third-party vendor.
- Bella: core engineer on the agent worker (TypeScript) of an AI chief of staff for a whole company: the runtime that plans, calls tools and replies on every turn, for staff on WhatsApp, Lark, Teams and web. Pluggable engines swap the reasoning backend without touching channels or tools, and every tool declares a minRole (admin, member or allowlisted) checked by a role gate before each call; a denial goes back to the model as a forbidden tool result, and the model tells the user.
- Messaging and observability: built the messaging engine between apps and chat platforms, and the observability behind it: per-turn cost, execution waterfalls, and a human-review loop that produces ground-truth datasets.

## Open source

- Mutation testers for Rust, Go and Dart ([rust_mutant](https://github.com/SulthanZahran1/rust_mutant), [gopher_mutant](https://github.com/SulthanZahran1/gopher_mutant), [dart-mutant](https://github.com/SulthanZahran1/dart-mutant)). Coding agents write a lot of tests; mutation testing plants small bugs and checks whether the tests notice; rust_mutant then compares the LLVM IR of surviving mutants to mark equivalent ones. rust_mutant is on [crates.io](https://crates.io/crates/rust-mutant).
- honjang: an English–Korean voice translator that picks the speech level for who you're talking to: 해요체 or 합쇼체, plus an auto mode that chooses between them. It waits for Deepgram's end-of-utterance signal (1.2 s of silence), translates the whole reply, then speaks it clause by clause; streaming LLM tokens into TTS is the planned next step (ADR-0004). [Live demo](https://honjang.zahranm.cloud), [source](https://github.com/SulthanZahran1/honjang).

Skills. AI: AI agents, tool use and MCP, RAG, LLM observability, voice pipelines, evals and mutation testing. Languages and infrastructure: Rust, Go, TypeScript, Python, C#, SQL, PostgreSQL, MongoDB, Redis, Docker, Traefik.

## Background

**Software Engineer, smart factory. LG Sinarmas Technology Solutions, Karawang, Apr 2024 to now.**

- MES, real-time dispatching (RTD) and AMHS integration for an EV-battery plant: C# / .NET, WPF, SQL Server and Oracle. RTD rules decide which vehicle moves which material, where; SECS/GEM and PLC handshakes connect tools to the MES.
- Wrote a Python + SQL log analyzer that replays millions of AMHS events as timing diagrams for incident diagnosis.
- Built a Korean–English RAG pipeline (LangChain, PostgreSQL) over factory documentation, and automated monitoring across the plant's IoT sensor web interfaces.

**AMHS Integration Specialist. LG Energy Solution Ochang, South Korea, Aug to Oct 2025.** Integration-tested AGV, conveyor and stocker subsystems against the MES before production, and fixed the integration bugs in code.

Before that:

- Instrumentation Engineer, PT Polychemie Asia Pacific Permai (Jan to Mar 2024): prototyped an ADC-based monitoring rig so temperature and flow could be logged on a PC instead of by hand; evaluated DAQ, PLC and Arduino options and wrote the plotting GUI.
- Research Assistant, software, FAAN Laboratory, Physics, Universitas Indonesia (Oct 2023 to Mar 2024): built the lab's MATLAB spectroscopy app, with real-time acquisition, peak detection and fitting, Savitzky–Golay filtering and Tauc plots.
- PLC Intern, PT Sugitama Intiarto (Jun to Jul 2022): first contact with industrial control software.
- B.Sc. Physics, Instrumentation, Universitas Indonesia (Aug 2019 to Jul 2023): IndySCC22 student cluster competition (HPC), 4th place, the only team from Southeast Asia; programming lead for the UI Robotics Team at KRTMI.

## How to use this site

Use this page for identity and context, the [home page](https://zahranm.cloud/) for interactive work demos, experience and the index of public projects, the [contact page](https://zahranm.cloud/contact) for direct communication, and the [CV (PDF)](https://zahranm.cloud/cv.pdf) for a one-file summary. The recruiter copilot is at [zahranm.cloud/recruit](https://zahranm.cloud/recruit). Agents can start with [llms.txt](https://zahranm.cloud/llms.txt) or [agent-instructions.md](https://zahranm.cloud/agent-instructions.md).

## Scope

This is a personal project hub, not a general-purpose software service. It does not offer an API, account system, paid checkout, MCP server, or automated support channel. Employer work is described, not linked. Follow the linked project site for the capabilities and terms of each individual project.

## Sitemap

- [Home](https://zahranm.cloud/)
- [Contact](https://zahranm.cloud/contact)
- [Privacy](https://zahranm.cloud/privacy)
- [llms.txt](https://zahranm.cloud/llms.txt)
