# Kiro Usage

Kage Crew is being built with Kiro for Kiroween 2026's Frankenstein category. Specs keep the one-month MVP aligned: a bounded multi-agent coordinator combines read-only public-repository checks with active testing against a separate hosted lab.

## Spec-driven scope

Requirements and design live in `.kiro/specs/kage-crew/`. They define the Repo Scout, Code Security, Playwright Web Attack, AI/RAG Red Team, and Evidence Verifier roles; the fixed workflow graph; input/target limits; finding evidence; graph behavior; and hosted deployment.

## Agentic security behavior

The coordinator dispatches fixed specialist workers and tracks their progress. The AI/RAG worker may use the target's observed responses to select a bounded follow-up prompt. Playwright attacks only the hosted lab; repository code is never executed. Deterministic verifiers decide whether findings are confirmed. The LLM does not grade its own success. Attempts, time, and model usage are capped.

## Honest MVP boundaries

The hosted demo accepts a public GitHub repository for read-only source checks and directs active attacks only to the separate lab. Its AI tests follow OWASP Top 10 for LLM Applications 2026: LLM01:2026 is the attack class, and LLM02:2026 is noted only when the canary is exposed. See `docs/OWASP_MAPPING.md`. The project does not claim complete OWASP coverage. The model key is a server-side deployment secret; the judge does not provide credentials. ElevenLabs narration is optional, while typed controls and visible results remain available.

## Kiro practices

Use Kiro specs for product constraints and interface behavior, and iterative agent-assisted development for worker boundaries, attack prompts, and response handling. Keep decisions and limitations documented. The coordinator and named specialist workers are MVP requirements; extra agents, hooks, and MCP integrations are not.
