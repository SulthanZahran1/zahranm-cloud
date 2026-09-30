---
title: Sulthan Zahran Ma'ruf, AI engineer
description: AI engineer in Indonesia building AI agents and the infrastructure they need in production: tool gateways, observability, and tests for agent-written code.
canonical_url: https://zahranm.cloud/
last_updated: 2026-09-30
---

# Sulthan Zahran Ma'ruf, AI engineer: I build AI agents that do real work

Agents that act on real accounts, the gateway they act through, and the console that shows what they did.

The home page opens with a simulated agent console, modelled on the observability console built for Bella. A visitor picks a request (for example "Move my 3pm"), watches the execution trace of model and tool-via-gateway spans with latency, tool calls, tokens and cost, reads the agent's reply, and marks the turn correct or wrong, as a human reviewer would.

## Outcomes

- Rp50B+ in B2B revenue generated with the messaging engine and monitoring I built.
- 8 weeks to replace a paid integrations vendor with our own agent tool gateway.
- 50 h/day of person-hours saved by one internal app I shipped at LG.
- 1 of 4 engineers selected for a two-month deployment to a battery plant in Korea.

## Selected work

Each tile on the home page is a small working model of something I built. Employer systems are simulated; personal projects link to source.

### The gateway agents act through (sambungapi)

Metatech, primary engineer. Private.

Agents call one API; the gateway holds every OAuth token encrypted, unlocks it only for the call, and talks to the provider. Built in-house in 8 weeks to replace a paid vendor.

- Demo: a diagram of agent → gateway (encrypted vault) → seven providers; clicking a provider animates a tool call and prints its log.
- Stack: Rust, axum, PostgreSQL, OAuth 2.0, durable outbox, multi-tenant.

### An AI chief of staff for a whole company (Bella)

Metatech, core engineer. Private.

Staff message it on WhatsApp, web or Lark; it handles email, meetings and cross-department updates within each person's permissions. I work on the engine that runs every turn and built the monitoring behind it.

- Demo: the request flow (WhatsApp, Lark or web → worker plans and calls tools → gateway to Gmail, Calendar, Lark → console with cost, trace and human review). The console at the top of the page simulates it.
- Minutes to catch failures with the monitoring I built.
- Rp50B+ B2B revenue, together with the messaging engine I built.

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

- Demo: an animated loop of AGVs around a track labelled "8 lines · 24/7, EV-battery production".
- 50 h/day person-hours saved by one of 8+ internal apps.
- 3 h → 15 min IoT sensor monitoring cycle, automated.
- Days → minutes incident diagnosis with the log analyzer I wrote.
- 15+ issues fixed at the Korean plant before production started.
- Stack: MES, RTD, AMHS, C# / .NET, SQL Server, Python, KO–EN RAG.

## Experience

On the home page the career is drawn as a clickable timing diagram. Lanes, top to bottom: AGENTS (Metatech), FACTORY (LG Sinarmas, including the Korea deployment), LAB (FAAN Lab), FIELD (PLC internship, instrumentation engineer), EDU (B.Sc. Physics).

- **Software Engineer, AI agents.** Metatech, part-time, remote. Apr 2026 to now.
  - Built sambungapi, the in-house gateway our agents use to act on Google, Microsoft, GitHub, Slack, Notion, Linear and Jira. It replaced a paid vendor in 8 weeks.
  - Core engineer on Bella, an AI chief of staff that works across WhatsApp, Gmail, Calendar and Lark.
  - Built the messaging engine that moves messages between apps and chat platforms, and the monitoring that catches failures in minutes. Together they generate Rp50B+ in B2B revenue.
- **Software Engineer, smart factory.** LG Sinarmas Technology Solutions, Karawang. Apr 2024 to now.
  - MES, real-time dispatching and material handling for an EV-battery plant running 8 lines, 24/7.
  - Shipped 8+ internal apps; one saves 50 person-hours a day.
  - Built a log analyzer that cut incident diagnosis from hours or days to minutes, and a Korean–English RAG over factory documentation.
  - Designed the team's onboarding and trained 15+ engineers.
- **AMHS Integration Specialist.** LG Energy Solution Ochang, South Korea. Aug to Oct 2025.
  - One of four engineers selected for a two-month deployment.
  - Caught and fixed 15+ integration issues across AGVs, conveyors and stockers before production.
- **Instrumentation Engineer.** PT Polychemie Asia Pacific Permai. Jan to Mar 2024.
  - Prototyped digital temperature and flow monitoring for a plant where readings were written down by hand.
- **Research Assistant, software.** FAAN Laboratory, Physics, Universitas Indonesia. Oct 2023 to Mar 2024.
  - Built the lab's spectroscopy analysis app, improving the synthesis–testing cycle by ~800%.
- **PLC Intern.** PT Sugitama Intiarto. Jun to Jul 2022.
  - First contact with industrial control software.
- **B.Sc. Physics, Instrumentation.** Universitas Indonesia. Aug 2019 to Jul 2023.
  - 4th place at IndySCC22 (supercomputing), the only team from Southeast Asia.
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
