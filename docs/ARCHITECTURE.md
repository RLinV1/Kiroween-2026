# Kage Crew Architecture (MVP)

## Product shape

Kage Crew's centerpiece is one bounded agentic prompt-injection attack against a bundled AI chat website. A planner proposes a small sequence of attack strategies, Playwright sends prompts through the visible UI, and an evaluator checks whether a planted canary secret appears in the response. The agent may adapt its next attempt based on the observed response, within a strict attempt and time budget.

The same bundled demo includes source code for a small static scan and a couple of conventional browser checks. These support the security-sweep story; they do not become a general-purpose scanner in the MVP.

## Run flow

1. Judge opens the public Cloud Run demo and starts the bundled scan.
2. Code checks inspect the sample source. The browser agent uses Playwright against the bundled same-origin AI chat target.
3. The attack agent selects or generates an initial prompt-injection attempt, observes the target response, and can choose a bounded follow-up.
4. A deterministic evaluator declares a breach only when the synthetic canary value is present. Store and show the prompt, response, target page, and check outcome.
5. The graph shows observed source files and browser pages. Ghosts travel to nodes as work runs; findings pulse red. The blue-team demo applies a prepared mitigation, captures the ghost, turns the node blue, then retests to animate a shield holding or a breach persisting.
6. ElevenLabs narration can announce progress and findings if configured. Typed controls and text status remain the default path and work without voice credentials.

## Safety and limits

- Public demo runs only against its bundled, intentionally vulnerable target. It has no arbitrary URL input.
- Keep prompt count, response size, runtime, browser navigation, and scan concurrency bounded.
- The sample's secret is synthetic and contains no real credentials or private data.
- The model proposes attacks; Playwright executes only against the fixed sample route; deterministic evaluation decides whether the canary was exposed.
- Map prompt injection to relevant OWASP Top 10 for LLM Applications guidance and conventional findings to web application categories where relevant. The demo does not claim full coverage or that a retest proves overall security.

## Hosted deployment

Use one version-pinned Docker service on Google Cloud Run for the UI/API, sample target, and matching Playwright Chromium runtime. Include health checks and one-at-a-time scans. Validate a real hosted Playwright attack in the first two days, before graph and animation work. Keep a local Docker flow for the same sample and scan.

## MVP components

- **Frontend:** React/Vite graph, findings panel, progress, typed start/retest controls, reduced-motion support.
- **API and runner:** FastAPI endpoints that start a bounded scan and return progress/results for the bundled target.
- **Code checks:** a short deterministic roster over the bundled sample source.
- **Browser agent:** Playwright navigation and interaction limited to the sample origin.
- **Attack agent:** one LLM-backed prompt-injection loop, hard-limited to a few attempts; the LLM proposes payloads and next steps, not verdicts.
- **Target app:** small deliberately vulnerable chat page with a synthetic canary and a toggleable mitigation for the retest demo.
- **Graph:** files/pages and relationships observed during scanning; no invented data-flow edges.
- **Voice:** optional ElevenLabs TTS narration; STT/voice commands are stretch.

## Model credentials

Agentic payload generation requires model access. For the judge-facing deployment, use a developer-configured server-side provider credential stored as a Cloud Run secret and enforce a strict request/token budget. Never expose it to the browser or log it. Keep the app's deterministic code and browser checks usable when model access is absent; clearly disable the agentic attack in that case. Do not require judges to connect their own accounts.

## Deferred

Arbitrary user targets, uploads, auth/tenancy, multi-provider support, RAG, MCP tool discovery, multi-agent swarms, broad OWASP checks, automatic code changes, persistent histories, and live threat intelligence.
