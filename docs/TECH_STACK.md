# Tech Stack

## Frontend
- React 18 + Vite + TypeScript -- app shell, repo-graph UI, findings console, voice/text bar.
- Tailwind CSS -- haunted dark aesthetic.
- Framer Motion -- ghost and node animations (ghosts crawling edges, breach pulses, exorcism glows).
- React Flow -- the repo-graph visualization. Nodes are files / modules / endpoints; edges are data and control flow; ghost attackers animate along edges. The centerpiece of the demo.
- Zustand -- local UI state.
- TanStack Query -- server/async state and polling attack jobs.
- Howler / Web Audio -- ElevenLabs TTS audio playback.
- @elevenlabs/client-sdk -- ElevenLabs STT/TTS streaming; Web Speech API as a zero-cost STT fallback.
- Text input -- a typed command box alongside voice, so you can speak or type to command the crew.

## Backend
- Python 3.11 + FastAPI -- async API and orchestration.
- SQLAlchemy ORM + Alembic -- schema and migrations.
- SQLite (MVP/dev) -> Postgres (final/deploy) -- tenant_id row scoping for isolation.
- cryptography (Fernet) -- encrypting user LLM keys at rest.
- Pydantic v2 -- request/response schemas.
- httpx -- async calls to LLM providers and MCP.
- MCP server (Python, MCP SDK / fastmcp) -- exposes the ghost-crew attack tools.
- LLM provider adapters -- openai, anthropic, google-generativeai; optional LiteLLM for normalization.
- Playwright -- the live runtime agent. Drives the target app like a human attacker (navigate, click, type, read DOM and responses). This is the "runtime behavior" limb that makes the chimera real (source vs runtime).
- Execution -- FastAPI BackgroundTasks (MVP) -> Celery/RQ + Redis (final) for attack jobs.

## Voice
- ElevenLabs TTS -- narrates orchestration, reads findings, voices the target leaked responses.
- ElevenLabs STT (Scribe) -- voice commands that orchestrate the ghost crew.
- Web Speech API -- free in-browser STT fallback.
- Text input -- typed commands as a first-class alternative to voice.

## Data & external services
- BYO user LLM key (OpenAI / Anthropic / Gemini) -- powers the target, orchestrator, and attacker.
- ElevenLabs API.
- Threat-intel -- bundled local OWASP LLM Top 10 / MITRE ATLAS / CWE JSON (MVP); NVD API + OSV (stretch).

## Deployment
- Frontend -- Vercel or Cloudflare Pages (static, free).
- Backend -- Render or Koyeb (Python, free tier); Fly.io as an alternative. Railway is avoided given the October 2026 breach.
- Playwright runtime -- bundled Chromium; the backend runs the browser agent headless (or via a browser service for deployment).
- Secrets as environment variables (master encryption key, ElevenLabs key); never committed.

## Why these choices
- React Flow + Framer Motion make the repo-graph-with-ghosts the visual hook judges remember.
- Playwright makes the "live runtime" limb real -- the chimera is source analysis vs a browser that actually drives the app, not two text-based scanners.
- MCP at runtime makes the agentic loop real (the LLM picks attack tools via MCP), not a scripted pipeline.
- BYO LLM keeps the tool useful to any developer's stack; the provider-adapter layer keeps it vendor-agnostic.
- Python + FastAPI fits security and agent orchestration, and matches the stack the 2025 Frankenstein winner used.
