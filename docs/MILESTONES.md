# Kage Crew MVP (two weeks)

## Product goal

Kage Crew is a web security demo centered on an agentic prompt-injection attack. An attack agent adapts a bounded sequence of prompts while Playwright drives a bundled AI website and captures evidence. Code checks, a graph of inspected files/pages, and a retest loop support the attack story.

The MVP is a bounded sweep. Map the prompt-injection finding to relevant OWASP Top 10 for LLM Applications guidance, and conventional code/browser checks to the OWASP Top 10 for Web Applications where relevant. Do not claim full coverage of either list or guarantee security.

## Demo design

The submission URL opens a working hosted demo. The judge clicks **Start demo attack** and sees a prompt-injection agent choose attempts, Playwright submit them to the bundled vulnerable AI chat page, and a bounded follow-up adapt to the observed response. The model proposes attacks; a deterministic canary check verifies whether the synthetic secret leaked. A small code-check ghost scans the bundled source in the same run. Findings show the page/file, prompt or check, observed evidence, a relevant OWASP category, and a suggested fix. A prepared mitigation can be applied and the same attack rerun.

### Deployment shape

- Package the web app, API, bundled sample site/source, and Playwright Chromium in a version-pinned Docker image. Serve the UI and API from one web service to keep the demo path simple.
- Use Google Cloud Run as the initial deployment target. It accepts a container image and provides a service URL for the judge demo. Confirm the browser launches and the sample scan completes there before building the graph experience; Azure App Service Linux containers are the fallback if Cloud Run fails this proof.
- Add a health endpoint and a visible scan status/error. Keep scans single-worker and bounded for predictable resource use.
- Treat only the bundled sample as a permitted target in the public demo. Do not expose an arbitrary URL input to public visitors.
- Keep a local Docker path so the same image and scan flow can be reproduced during development.

The hosted demo scans only this bundled target. It requires no judge API key, account, code upload, or access to the judge's computer. Keep local development supported. Do not allow public visitors to scan arbitrary websites.

## Two-week plan

### Days 1–2 — prove the hosted browser path

- Build a minimal Docker image and deploy a health-check app to Cloud Run immediately.
- Bundle a small intentionally vulnerable website and its source code.
- Run Playwright against the deployed sample from the hosted scan service; pin matching Playwright/browser versions and verify Chromium starts in that environment.
- Return one real Playwright prompt attempt and response, one deterministic canary evaluation, and one real code finding with evidence.

**Gate:** if hosted Playwright is not working by the end of day 2, try running the scanner and sample app in the same Docker service and verify again. If it still fails, choose another Docker host before adding features. Do not build graph polish before this path works.

### Days 3–5 — agentic attack and findings

- Add the LLM-backed attack loop: generate a prompt-injection attempt, observe the response through Playwright, and choose a bounded follow-up based on that response.
- Limit the attack to a few attempts against the bundled AI route, with request/time budgets and a synthetic canary. The deterministic evaluator alone marks a confirmed leak.
- Add one or two deterministic code checks and a safe browser observation to support the broader sweep.
- Show findings with file/page, prompt/action, observed evidence, applicable OWASP LLM or web application mapping, and suggested fix.

### Days 6–8 — graph crew

- Render the sample's source files and observed website pages as graph nodes.
- Show code and Playwright agents moving across the nodes they actually inspect.
- Highlight findings on their source or page node and show progress/error states.
- Animate ghosts spawning at scan start and traveling to checked nodes. Pulse a node red when a finding appears and reveal its evidence marker.

### Days 9–10 — retest and experience

- Add a prepared prompt-defense change and per-attack retest that demonstrates the agent no longer extracts the canary (or still succeeds if the issue remains).
- Show the blue-team capture beam containing a finding ghost and turning its node blue; on retest, animate a shield holding when fixed or the ghost breaking through when the issue remains.
- Add basic visual polish, clear empty/error states, and a simple landing/start-scan path.
- Add ElevenLabs narration if credentials and integration are ready; keep text status and controls complete without audio. Voice commands are stretch.

### Days 11–14 — stabilize and submit

- Test the complete flow from the public URL in a clean browser and on another device/network.
- Fix deployment, browser, timeout, and first-run issues; keep a short recorded walkthrough as backup.
- Finish setup instructions, demo script, and category coverage notes.

## Must-have / cutline

Must ship: public demo URL, bundled sample and source, one real hosted Playwright run, at least one real code check, graph with visible agents, evidence-backed findings, and a working retest example.

Cut first if time runs short: voice commands, ElevenLabs narration, extra vulnerability checks, complex graph edges, custom uploads, multiple agents, and provider sign-in. Keep the one bounded LLM attack agent because it is the centerpiece. Keep animations brief and respect reduced-motion settings.

## Scope limits

- No complete OWASP Top 10 coverage claim; distinguish LLM application mappings for prompt injection from web application mappings for conventional checks, and mark uncovered categories.
- No arbitrary public target scanning, unrestricted crawling, destructive tests, or unauthorized use.
- No multi-tenant accounts, arbitrary repository uploads, multiple LLM providers, RAG features, live threat feeds, or automatic code edits.
- The LLM proposes attacks and adapts to responses; it does not decide whether an attack succeeded. A deterministic canary match confirms the finding.
- Store the LLM credential only server-side as a Cloud Run secret, enforce a strict usage budget, and disable the agentic attack clearly when no key is configured. The public judge flow must use the project-configured key, not ask judges to bring one.

## Acceptance demo

From the submission URL, a judge starts the bundled attack without setup or credentials. They see the agent adapt a prompt based on a Playwright-observed response, inspect the prompt/response and deterministic canary verdict, follow the agents in the graph, apply the prepared mitigation, and rerun the attack to see whether the leak persists. The core path works without voice and takes about three minutes.
