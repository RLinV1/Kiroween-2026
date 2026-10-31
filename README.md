# Kage Crew

Kage Crew is a graph-based security workbench where a crew of agents scans code and uses Playwright to test a running website. Ghosts move through the files and pages they inspect, then surface findings with evidence. ElevenLabs gives the crew voice commands and narration.

Built for **Kiroween 2026**, in the **Frankenstein** category: Kage Crew brings static code analysis, live browser testing, an animated graph, and voice interaction together in one security tool.

## Why the name

“Kage” (影) means “shadow” in Japanese. It evokes the ghost agents quietly tracing a codebase and probing a site's behavior. “Crew” describes their coordinated roles: some inspect code, while others use a browser to test the running site. The name gives the agents a distinct identity and ties directly to the haunted graph experience.

## The MVP

The one-month MVP is a bounded security sweep informed by the OWASP Top 10 for web applications. It maps only the categories covered by its implemented checks; it does not claim complete OWASP coverage or guarantee that a site is secure.

- **Code scan:** deterministic checks for a small set of clear issues, with file and line evidence where available.
- **Playwright scan:** safe, bounded navigation and checks against a running website, with the page, action, and observed result recorded.
- **Ghost crew graph:** files, modules, and observed pages appear as nodes; animated agents show scan progress and findings.
- **Evidence and retesting:** findings include relevant OWASP mapping and remediation guidance, with a way to rerun a check after a fix.
- **Voice:** ElevenLabs provides narration and a small set of commands; typed controls remain available.
- **Working demo URL:** the deployed app includes a bundled sample project and intentionally vulnerable website so judges can try the core loop without uploading their own code. Local development and testing are supported too.

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
- [Requirements and design specs](.kiro/specs/specter/)

## License

MIT — see [LICENSE](LICENSE).
