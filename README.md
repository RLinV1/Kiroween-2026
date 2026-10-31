# Kage Crew

An AI red-teaming and blue-teaming workbench for the agentic-AI era. A Frankenstein that stitches incompatible security paradigms into one autonomous crew that attacks your AI app, then defends it.

Built for **Kiroween 2026** (Frankenstein category).

“Kage” (影) means “shadow” in Japanese. It fits the ghost agents that move through a codebase and probe a running site, while “Crew” suggests a coordinated team of specialized scanners. Together, Kage Crew gives the security agents a memorable identity that supports the project's haunted graph and makes the work visible to developers.

## Why this matters now

Over the last few weeks alone, attackers have targeted AI infrastructure directly. The October 2026 Railway breach went after AI provider API keys as part of what Railway called a coordinated campaign against AI inference providers; a poisoned LiteLLM release hit the AI tooling ecosystem in the same window. As developers move to agentic development (AI agents, MCP servers, RAG pipelines), they inherit a brand-new attack surface that traditional security scanners were never built to test.

## What Kage Crew does

Kage Crew is an AI security workbench where code-scanning and Playwright agents inspect a project and its running website. Ghosts traverse a graph of observed files and pages, report evidence-backed findings, and narrate their work through ElevenLabs.

- **Red team (attack):** the ghost crew attacks your system across stitched paradigms, iterating until it breaks or an attack budget is exhausted. Every finding ships with proof.
- **Blue team (defend):** each finding surfaces a suggested mitigation. You apply it, and the orchestrator re-invokes the relevant ghost to confirm the fix holds. Exorcised (fixed) vs. persists (still vulnerable).

## Why Kage Crew is a Frankenstein

Kage Crew stitches together different security approaches that normally live in separate tools:

1. **Static source-code analysis** (deterministic, no AI) -- what the code IS.
2. **Live runtime driving via Playwright** (a browser agent that actually clicks, types, and reads responses) -- what the app DOES.
3. **Generative adversarial LLM attacks** (probabilistic, creative, human-like) -- what an attacker could invent.
4. **Voice OR typed orchestration** (conversational human intent driving a relentless machine swarm) -- speak or type to command the crew.
5. **A static repo-graph animated by a living swarm** (spatial structure vs behavioral attack) -- the code as a map, made alive by ghosts.
6. **Red team to blue team** -- attack and defend as adversarial opposites in one loop.

The incompatible pairs are real:
- Source-as-text vs runtime-as-living-behavior (a scanner that reads code vs a browser that drives the app).
- Deterministic rules vs generative AI (reliable known-pattern detection vs creative novel-exploit generation).
- Human conversation vs autonomous machine swarm (you speak or type; the crew executes relentlessly).
- Attack vs defend (red and blue team in one tool).

No single approach is a real red team. Stitched and cross-checked -- the deterministic engine confirms the generative findings (kills false positives), and the generative engine probes what the static engine flags (goes deeper) -- together they are. That is the "unexpectedly powerful."

The Frankenstein theme is structural, not a skin: the target AI system is the "creature"; the paradigms are incompatible "limbs" stitched onto it; a prompt-injection vulnerability is a "ghost" that corrupts it; applying a mitigation "exorcises" the ghost. The attack-to-defend loop is the architecture.

## The agentic core

You speak or type a command. ElevenLabs (or text) turns it to intent. An LLM orchestrator interprets it and uses **MCP** to discover and invoke the ghost-crew attack tools, deciding which to call and in what order. The ghosts attack -- by reading the code (static), driving the live app (Playwright), and generating creative exploits (LLM) -- findings come back tagged, and ElevenLabs narrates the whole thing. MCP is load-bearing at runtime, not just during development.

```
speak/type -> ElevenLabs STT (or text) -> orchestrator LLM -> MCP tool selection -> ghost execution (static + Playwright runtime + generative) -> findings -> ElevenLabs TTS narration
```

## What you see

The target app renders as a **repo graph** (files / modules / endpoints as nodes, data and control flow as edges). Ghost sprites crawl along the edges toward target nodes; on a successful breach the node pulses and reveals the proof (the winning payload plus the target's leaked response) and its real classification (OWASP LLM Top 10 / MITRE ATLAS / CWE). Exorcising a ghost (a blue-team fix) turns the node blue.

## Attack surface

Primarily **AI vulnerabilities**: prompt injection, jailbreaks, RAG data exfiltration / poisoning, agent / tool hijacking, insecure output handling. Also supports **classic application vulnerabilities** (XSS, SQL injection, hardcoded secrets, weak crypto) as a secondary surface.

## Tech stack

**Frontend:** React + Vite + TypeScript, Tailwind CSS, Framer Motion, React Flow (the repo-graph), Zustand + TanStack Query, the ElevenLabs client SDK (Web Speech API fallback).

**Backend:** Python + FastAPI, SQLAlchemy + Alembic, SQLite (MVP) to Postgres (final), cryptography (Fernet key vault), an MCP server exposing the ghost-crew tools, LLM provider adapters (OpenAI / Anthropic / Gemini), and **Playwright** as the live runtime agent.

**Voice:** ElevenLabs TTS (narration + target speech) and STT (voice commands), plus typed input. See [docs/TECH_STACK.md](docs/TECH_STACK.md).

## Kiro usage

Built with Kiro and demonstrating deep, honest usage: spec-driven development for the UI, vibe coding for the red-team / agent logic (the inverted pattern, documented), an agent hook that auto-generates a new attack-class / check rule when added to a spec, MCP for both development-time reference and the runtime agentic loop, steering docs for consistency, property-based testing for detection rules, and multi-agent orchestration (the ghost crew). See [docs/KIRO_USAGE.md](docs/KIRO_USAGE.md).

## Documentation

- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)
- [docs/TECH_STACK.md](docs/TECH_STACK.md)
- [docs/MILESTONES.md](docs/MILESTONES.md)
- [docs/KIRO_USAGE.md](docs/KIRO_USAGE.md)
- [MVP brainstorming](docs/MVP_BRAINSTORM.md) and [four-week scope](docs/MILESTONES.md)
- `.kiro/specs/specter/` -- requirements and design

## License

MIT -- see [LICENSE](LICENSE).
