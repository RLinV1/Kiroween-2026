# Kage Crew

Kage Crew is a graph-based security workbench coordinated by a bounded multi-agent workflow. It scans a public GitHub repository for source issues and sends a browser attack crew against a separate hosted security lab. Specialist workers check code, test SQL injection and XSS with Playwright, and probe the lab's RAG chat for prompt injection and data leakage. A deterministic verifier checks evidence before findings appear in the ghost graph.

Built for **Kiroween 2026**, in the **Frankenstein** category: Kage Crew brings repository analysis, browser security testing, adaptive AI red teaming, deterministic verification, and a graph-based ghost crew together in one security tool.

## Why the name

“Kage” (影) means “shadow” in Japanese. It evokes the ghost agents quietly tracing a codebase and probing a site's behavior. “Crew” describes their coordinated roles: some inspect code, while others use a browser to test the running site. The name gives the agents a distinct identity and ties directly to the haunted graph experience.

## The MVP

The one-month MVP uses a public GitHub link for read-only source analysis and a separate, hosted lab for active attacks. Its UI shows all ten OWASP LLM risks in a coverage map and marks checks that are not implemented. The lab includes bounded SQL injection and XSS checks plus an actual RAG chat that can ingest one Markdown or text document for prompt-injection and leakage testing. Verified canary exposure maps to **LLM02:2026 Sensitive Information Disclosure**. See [our mapping and evidence rules](docs/OWASP_MAPPING.md); displaying the full list does not imply full coverage.

- **Repo scan:** deterministic secret, dependency, and common source checks scan a public GitHub snapshot without executing it. Findings include file/line evidence when available, and secret values are redacted.
- **Multi-agent orchestrator:** a bounded coordinator dispatches the Repo Scout, Code Security worker, Playwright Web Attack crew, AI/RAG Red Team, and Evidence Verifier. The AI proposes/adapts attacks; deterministic checks decide whether findings are confirmed.
- **Live attack lab:** Playwright checks SQLi and XSS against controlled routes and drives the lab's RAG chat. A judge can ingest one document during the demo; retrieval evidence and a synthetic canary make leakage results verifiable.
- **Ghost crew graph:** coordinator, workers, repository files, lab pages, retrieved chunks, findings, and evidence appear as nodes. Ghostbusters capture a finding ghost only after a successful verified retest.
- **Evidence and retesting:** findings include relevant OWASP mapping and remediation guidance, with a way to rerun a check after a fix.
- **Voice:** ElevenLabs narration is a stretch feature after the attack loop works; typed controls remain available. Voice commands are deferred.
- **Working demo URL:** judges can submit a public GitHub URL and run attacks against the hosted lab without installing the project or supplying personal API credentials. Local Docker development uses the same lab.

Active attacks are restricted to the hosted lab. Repository analysis is read-only and bounded; unsupported languages or checks are shown as such. Arbitrary live-site attacks and a local-development CLI are future work. ElevenLabs narration is optional and cannot block the core scan.

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
