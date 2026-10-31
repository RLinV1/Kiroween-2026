# Kage Crew MVP Design

## Experience

The judge opens the Cloud Run URL and clicks **Start demo attack**. The graph shows code and browser ghosts moving through a bundled sample project and its AI chat page. The central attack ghost sends a prompt-injection attempt through Playwright, reads the visible response, and chooses a limited follow-up based on what it observed. A deterministic canary check confirms or rejects a leak.

Findings show the file or page, the prompt/check, observed response or source evidence, applicable OWASP category, and suggested mitigation. The judge applies a prepared prompt-defense change and clicks **Retest**. A capture-beam animation contains the ghost; the node turns blue. The ghost revisits the target: a shield holds if the canary stays hidden, or the ghost breaks through if it leaks again.

All actions are available as typed controls. ElevenLabs narration can announce milestones and findings if configured. The scan remains usable without audio.

## Architecture

- **Web app:** React/Vite/TypeScript interface with graph, findings, scan progress, and retest controls.
- **API:** FastAPI starts one bounded scan at a time and returns status and evidence.
- **Target:** bundled intentionally vulnerable AI chat app and source. Its canary secret is synthetic.
- **Attack agent:** one LLM-backed loop proposes attacks and selects the next prompt from observed response context. A strict attempt/time/token budget caps the loop.
- **Browser runner:** version-pinned Playwright Chromium drives only the bundled target's same-origin UI.
- **Evaluator:** deterministic canary match determines attack success. The model never grades its own outcome.
- **Code checks:** a small deterministic roster scans the bundled source and reports file/line evidence.
- **Graph:** display source files and browser-observed pages. Edges represent only observed relationships. Attach attack/check status to relevant nodes.
- **Blue-team demo:** mitigation changes the sample target's prompt-defense behavior and lets the same attack run again.

## Deployment

Build one Docker image with the app, API, bundled target/source, and matching Playwright browser dependencies. Deploy to Google Cloud Run. Configure the app port, health endpoint, low concurrency, bounded request/runtime settings, and one scan at a time. Keep the model credential in a Cloud Run secret. Prove the hosted Playwright attack during days 1–2 before implementing the graph. Keep a local Docker run path using the same sample.

## Agent loop

1. Start from a small system-defined set of prompt-injection strategies.
2. Ask the model for one attack prompt within the current strategy and size limits.
3. Use Playwright to submit the prompt to the bundled AI chat page and capture its response.
4. Check the response for the synthetic canary with deterministic matching.
5. If not exposed and attempts remain, send a short response summary to the model to select or phrase one bounded follow-up.
6. Save the sequence and evidence. Stop at the attempt, time, or usage limit.

## Safety and failure behavior

No user-controlled target URL is accepted in the public demo. The runner rejects navigation outside the bundled origin. API/model errors, timeouts, and unavailable voice services become visible status messages. Without a configured model secret, deterministic code/browser checks still run and the interface marks the agentic attack unavailable; it must not substitute scripted attempts while claiming autonomy.

## Animation direction

Ghosts spawn at the graph root and move along observed edges to checked nodes. A finding pulses red and reveals an evidence marker. The blue-team capture beam contains the ghost and changes the node to blue. During retest, animate a shield holding or the ghost breaking through. Keep animations brief, preserve clear text status, and honor reduced-motion settings.

## Deliberately deferred

Multi-agent planning, arbitrary URLs/repositories, user accounts, full OWASP coverage, multiple providers, RAG, MCP discovery, live threat feeds, broad vulnerability catalogs, code edits, persistent history, STT commands, and hosted multi-tenant operation.
