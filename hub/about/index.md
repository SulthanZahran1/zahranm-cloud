---
title: About Sulthan Zahran Ma'ruf, AI engineer
description: About Sulthan Zahran Ma'ruf, an AI engineer in Indonesia building agents and the infrastructure they need in production.
canonical_url: https://zahranm.cloud/about
last_updated: 2026-09-30
---

# About Sulthan Zahran Ma'ruf, AI engineer

Sulthan Zahran Ma'ruf is an AI engineer in Indonesia. He builds AI agents and the infrastructure they need in production: a tool gateway that lets agents act on real accounts, observability that shows what each turn did and what it cost, and mutation testing for the tests agents write. Before agents, he shipped smart-factory software for EV-battery lines, so he builds as if downtime is expensive.

## Agents and infrastructure

**Software Engineer, Metatech, part-time, remote (Apr 2026 to now).** AI agents, the observability tools that watch them, and the messaging infrastructure they run on.

- sambungapi (primary engineer): an in-house Rust gateway that lets agents act on real accounts (Gmail, calendars, chat apps) through OAuth-connected tools, replacing a layer rented from a vendor. A single static binary with its own /v1 API, hosted OAuth connect pages and a migration runbook for the agent platform; OAuth tokens are encrypted (AES-256-GCM) in the team's own Postgres instead of a vendor's. Eight weeks from empty repo to production gateway: 3,100+ commits, 326 test files.
- Bella (core engineer, team of several): an AI chief of staff across WhatsApp, Gmail, Calendar and Lark. Work centres on the worker that executes agent turns (2,000+ commits), plus most of the observability console: per-turn cost, an execution waterfall, and a human-review loop that turns real conversations into labelled ground-truth datasets.

## Open source

- Mutation testers for Rust, Go and Dart ([rust_mutant](https://github.com/SulthanZahran1/rust_mutant), [gopher_mutant](https://github.com/SulthanZahran1/gopher_mutant), [dart-mutant](https://github.com/SulthanZahran1/dart-mutant)), built to check the tests agents write. rust_mutant and dart-mutant run only the tests that cover a mutated line, and rust_mutant compares LLVM IR to throw out mutants that compile to identical code. rust_mutant 1.0.1 is on crates.io.
- [honjang](https://github.com/SulthanZahran1/honjang): a real-time English–Korean voice translator that tracks honorifics. Speech streams through recognition, an LLM and speech synthesis, with LLM output split at Korean clause boundaries so audio can start before the sentence is done.

## How he works

- Decisions go on paper. Every hard-to-reverse choice gets an ADR. On Bella the team keeps ~340, and he has written or revised 120+. Agents and new teammates both read the reasoning, not just the code.
- Test the tests. Agents write a lot of tests, so he measures whether those tests would catch a real bug. That is why he built mutation testers.
- Measure before you trust. On a factory floor a bad deploy stops a production line. Build the observability first, and don't trust anything until it's measured.

AI / agents: tool-use and MCP, RAG, agent harnesses, LLM observability, voice pipelines. Languages: Rust, Go, TypeScript, Python, C#, SQL. Infra: PostgreSQL, MongoDB, Redis, Docker, Traefik, Linux. Industrial: MES, AMHS, RTD, PLC, SECS/GEM, AGV integration.

## Background

**Software Engineer, LG Sinarmas Technology Solutions, Karawang (Apr 2024 to now).** Builds and integrates the software between the manufacturing execution system (MES), real-time dispatching (RTD) and the automated material handling system (AMHS) of an EV-battery plant: the vehicles, conveyors and stockers that move material between processes.

- Shipped 8+ internal applications; one saves 50 person-hours a day.
- Wrote the Timing Diagram Analyzer, which turns millions of material-handling log entries into a timing view: 4,120,953 entries analysed in 104 seconds; diagnosis down from hours or days to minutes; the whole team uses it.
- Built a Korean–English RAG pipeline for cross-language retrieval over factory documentation.
- Designed the team's onboarding program and trained 15+ software engineers.
- Liaison between Indonesian factory operations and the Korean engineering HQ.

**AMHS Integration Specialist, LG Energy Solution Ochang, South Korea (Aug to Oct 2025).** One of four engineers selected for a two-month deployment to integrate material handling before production. Fixed 15+ pre-production issues across AGVs, conveyors and stockers.

Before that:

- Instrumentation Engineer, PT Polychemie Asia Pacific Permai (Jan to Mar 2024). Project-based instrumentation work.
- Software Engineering Research Assistant, FAAN Laboratory, Physics, Universitas Indonesia (Oct 2023 to Mar 2024). Built lab software that improved the synthesis–testing cycle by roughly 800%.
- PLC intern, PT Sugitama Intiarto (Jun to Jul 2022).
- B.Sc. Physics, Instrumentation, Universitas Indonesia (Aug 2019 to Jul 2023). 4th place at IndySCC22, the only team from Southeast Asia. Programming lead for the KRTMI robotics contest, UI Robotics Team.

## How to use this site

Use this page for identity and context, the [home page](https://zahranm.cloud/) for case studies, experience and the index of public projects, and the [contact page](https://zahranm.cloud/contact) for direct communication. The recruiter copilot is at [zahranm.cloud/recruit](https://zahranm.cloud/recruit). Agents can start with [llms.txt](https://zahranm.cloud/llms.txt) or [agent-instructions.md](https://zahranm.cloud/agent-instructions.md).

## Scope

This is a personal project hub, not a general-purpose software service. It does not offer an API, account system, paid checkout, MCP server, or automated support channel. Employer work is described, not linked. Follow the linked project site for the capabilities and terms of each individual project.

## Sitemap

- [Home](https://zahranm.cloud/)
- [Contact](https://zahranm.cloud/contact)
- [Privacy](https://zahranm.cloud/privacy)
- [llms.txt](https://zahranm.cloud/llms.txt)
