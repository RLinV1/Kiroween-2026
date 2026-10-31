# Tech Stack (MVP)

## App and graph

- React, Vite, and TypeScript for the web app.
- React Flow for the file/page graph.
- Framer Motion for short ghost travel, finding pulse, capture beam, and retest shield animations. Respect reduced-motion preferences.

## Scan service

- Python 3.11 and FastAPI for the scan API and runner.
- Playwright with pinned Chromium to test only the hosted lab in the MVP.
- Bounded public GitHub snapshot intake; never execute the repository or install its dependencies.
- Deterministic secret, dependency, source, SQLi, and XSS checks with explicit supported-language and coverage status.
- A fixed coordinator workflow dispatches three ghost crews—Code Scout, Web Attack, and RAG Attack—and the Ghostbusters blue team. One configured LLM provider powers bounded, response-informed prompt injection; deterministic check rules decide whether findings are confirmed and whether Ghostbusters clear a finding after retest.
- The hosted lab contains vulnerable SQL/XSS routes and a RAG chat with actual retrieval and one-document Markdown/text ingestion. Synthetic canary checks verify disclosure.
- SQLite or in-memory state is sufficient for the single-demo scan; no account system or long-term result store in the MVP.

## Hosted demo

- Docker image includes the API/coordinator, built frontend, security lab, and matching Playwright Chromium dependencies.
- Google Cloud Run is the selected hosting target. Use a single container service initially, one scan at a time, a health endpoint, and bounded scan timeouts.
- Store the provider key as a Cloud Run secret; never put it in frontend code or logs. If unavailable, still run non-LLM checks and clearly report that agentic attacks are disabled.

## Voice

- ElevenLabs text-to-speech narration is a stretch feature after the hosted attack loop works.
- Voice recognition and commands are deferred. Typed controls and visible text status are required.
- The demo remains complete when ElevenLabs is not configured.

## Scope rationale

Use a fixed crew graph and bounded targets so the attack path is easy to explain and predictable: public repositories are analyzed read-only, while active browser attacks use the hosted lab. Follow OWASP LLM01:2026 Prompt Injection and LLM02:2026 Sensitive Information Disclosure when a canary is confirmed exposed. See [the mapping](OWASP_MAPPING.md). Conventional checks map to web application risks only where applicable; no mapping implies full coverage.
