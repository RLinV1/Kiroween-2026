# Kage Crew

Kage Crew is a graph-based security workbench with two complementary scan lanes: a conventional code scanner checks the project's source, while an agentic attack crew uses Playwright to test a bundled AI website. The attack agent adapts a bounded sequence of prompt-injection attempts and captures evidence. The graph shows both crews at work; ElevenLabs narration can bring findings to life.

Built for **Kiroween 2026**, in the **Frankenstein** category: Kage Crew brings static code analysis, live browser testing, an animated graph, and optional voice narration together in one security tool.

## Why the name

“Kage” (影) means “shadow” in Japanese. It evokes the ghost agents quietly tracing a codebase and probing a site's behavior. “Crew” describes their coordinated roles: some inspect code, while others use a browser to test the running site. The name gives the agents a distinct identity and ties directly to the haunted graph experience.

## The MVP

The two-week MVP follows the [OWASP Top 10 for LLM Applications 2026](https://genai.owasp.org/resource/owasp-genai-llm-top-10-2026/). Its UI shows all ten risks in a coverage map, clearly marking which are tested. The bundled attack targets **LLM01:2026 Prompt Injection**; a verified canary leak also maps to **LLM02:2026 Sensitive Information Disclosure**. See [our mapping and test evidence](docs/OWASP_MAPPING.md). Showing the full list does not imply full test coverage.

- **Regular code scan:** an independent, deterministic scanner checks the bundled project source for common security issues, with file and line evidence where available. It runs whether or not the LLM attack is enabled.
- **Agentic attack:** a single LLM-backed agent chooses and adapts prompt-injection attempts based on observed responses, with a strict attempt limit. A deterministic canary check decides whether the synthetic secret leaked.
- **Playwright scan:** drives the bundled AI website, submits attack prompts, and records the page, action, and observed response. A small number of safe browser checks supplement the attack.
- **Ghost crew graph:** files, modules, and observed pages appear as nodes; animated agents show scan progress and findings.
- **Evidence and retesting:** findings include relevant OWASP mapping and remediation guidance, with a way to rerun a check after a fix.
- **Voice:** ElevenLabs narration is a stretch feature after the attack loop works; typed controls remain available. Voice commands are deferred.
- **Working demo URL:** the deployed app includes a bundled sample project and intentionally vulnerable AI website so judges can try the core loop without setup. Local development and testing are supported too.

The hosted demo is restricted to the bundled target. Arbitrary site scans require authorization and clear scope. Voice and model-assisted extras may depend on external services; the core sample scan should remain usable without them.

## Scope and schedule

See [the brainstorming and design tradeoffs](docs/MVP_BRAINSTORM.md) and [the four-week MVP plan](docs/MILESTONES.md).

## Kiro usage

Kage Crew is built with Kiro through spec-driven development and documented implementation decisions. See [docs/KIRO_USAGE.md](docs/KIRO_USAGE.md).

## Documentation

- [Architecture](docs/ARCHITECTURE.md)
- [Tech stack](docs/TECH_STACK.md)
- [MVP brainstorming](docs/MVP_BRAINSTORM.md)
- [Four-week plan](docs/MILESTONES.md)
- [Kiro usage](docs/KIRO_USAGE.md)
- [Requirements and design specs](.kiro/specs/kage-crew/)

## License

MIT — see [LICENSE](LICENSE).
