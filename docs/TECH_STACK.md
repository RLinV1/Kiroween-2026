# Tech Stack (MVP)

## App and graph

- React, Vite, and TypeScript for the web app.
- React Flow for the file/page graph.
- Framer Motion for short ghost travel, finding pulse, capture beam, and retest shield animations. Respect reduced-motion preferences.

## Scan service

- Python 3.11 and FastAPI for the scan API and runner.
- Playwright with pinned Chromium version to drive the bundled AI chat website.
- Deterministic Python checks for a small set of source-code issues and browser observations.
- A bounded agent loop calls one configured LLM provider to propose prompt-injection payloads and choose a limited follow-up from observed responses. A deterministic canary matcher evaluates success.
- SQLite or in-memory state is sufficient for the single-demo scan; no account system or long-term result store in the MVP.

## Hosted demo

- Docker image includes the API, built frontend, bundled sample source/site, and matching Playwright Chromium dependencies.
- Google Cloud Run is the selected hosting target. Use a single container service initially, one scan at a time, a health endpoint, and bounded scan timeouts.
- Store the provider key as a Cloud Run secret; never put it in frontend code or logs. If unavailable, still run non-LLM checks and clearly report that agentic attacks are disabled.

## Voice

- ElevenLabs text-to-speech narration is a stretch feature after the hosted attack loop works.
- Voice recognition and commands are deferred. Typed controls and visible text status are required.
- The demo remains complete when ElevenLabs is not configured.

## Scope rationale

Use a single model provider and a fixed target so the agentic attack is easy to explain and the live scan is predictable. Follow OWASP LLM01:2026 Prompt Injection for the primary attack and LLM02:2026 Sensitive Information Disclosure when the canary is confirmed exposed. See [the mapping](OWASP_MAPPING.md). Conventional checks map to web application risks only where applicable; no mapping implies full coverage.
