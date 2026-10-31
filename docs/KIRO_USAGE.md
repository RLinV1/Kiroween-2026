# Kiro Usage

Kage Crew is being built with Kiro for Kiroween 2026's Frankenstein category. The project uses specs to keep the two-week MVP and its implementation aligned; the agentic prompt-injection attack is the central technical feature.

## Spec-driven scope

Requirements and design live in `.kiro/specs/kage-crew/`. They define the bundled target, bounded LLM attack loop, Playwright execution, deterministic canary evaluation, code checks, graph behavior, and hosted deployment checkpoint.

## Agentic security behavior

The attack agent uses the target's observed responses to select a bounded follow-up prompt. Playwright operates the target website, while a deterministic canary evaluator decides whether the synthetic secret was exposed. The LLM does not grade its own success. Attempts, time, and model usage are capped.

## Honest MVP boundaries

The hosted demo runs against one bundled target. Its prompt-injection test follows OWASP Top 10 for LLM Applications 2026: LLM01:2026 is the attack class, and LLM02:2026 is noted only when the canary is exposed. See `docs/OWASP_MAPPING.md`. The project does not claim complete OWASP coverage. The model key is a server-side deployment secret; the judge does not provide credentials. ElevenLabs narration is optional, while typed controls and visible results remain available.

## Kiro practices

Use Kiro specs for product constraints and interface behavior, and iterative agent-assisted development for attack prompts and response handling. Keep decisions and limitations documented. Add hooks, multiple agents, broader attack classes, or additional MCP integrations only if they are implemented and verified; they are not MVP promises.
