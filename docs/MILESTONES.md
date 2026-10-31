# Kage Crew MVP (two weeks)

## Product goal

Kage Crew is a web security demo with two complementary scan lanes: a conventional source-code scanner and a bounded agentic prompt-injection attack. The attack agent adapts prompts while Playwright drives a bundled AI website and captures evidence. A graph shows both crews, their findings, and the retest loop.

Follow the OWASP Top 10 for LLM Applications 2026 for the centerpiece: **LLM01:2026 Prompt Injection**, with **LLM02:2026 Sensitive Information Disclosure** when the synthetic canary leaks. Conventional code/browser checks may map to OWASP Top 10 for Web Applications where relevant. Record the test evidence and do not claim full coverage or guarantee security; see [the mapping](OWASP_MAPPING.md).

Show all ten OWASP LLM risks in a simple coverage map. Identify the tested categories and mark the rest **Not tested in this demo**; the coverage map is an honest guide to scope, not a claim of implementation.

## One-month overview

By the end of the month, deliver a judge-ready Kage Crew demo on Google Cloud Run. Its centerpiece is a real, bounded LLM attack agent using Playwright against the bundled AI chat target; the judge can watch the crew in the graph, inspect evidence, apply the prepared defense, and retest. Small source checks, OWASP mapping, and optional ElevenLabs narration round out the experience. The target remains fixed and synthetic so the demo is safe, affordable, and repeatable.

The first two weeks are for getting the complete core loop working and publicly deployed. Weeks three and four are for making that same loop reliable, understandable, and polished—not for expanding into arbitrary targets or a broad scanner.

| Period | Outcome |
|---|---|
| **Week 1** | Prove Cloud Run can run the bundled site and Playwright; build the bounded adaptive attack, canary verdict, and one code check. |
| **Week 2** | Complete the graph, evidence, prepared mitigation, and retest; reach a usable public demo URL. |
| **Week 3** | Harden the hosted flow, improve error states and animation/accessibility, and add ElevenLabs narration if the core path is stable. |
| **Week 4** | Test from a clean browser/device, control model spend, fix deployment issues, and prepare the submission/demo walkthrough. |

**End-of-month success:** a judge can open the URL without setup, watch the agent attack through Playwright, see a verified finding on the graph, apply the sample defense, and retest successfully in about three minutes.

## Demo design

The submission URL opens a working hosted demo. The judge clicks **Start scan** and sees a code ghost scan the bundled source for common issues while the attack agent sends prompt injections to the bundled vulnerable AI chat page through Playwright. The agent can adapt a bounded follow-up to the observed response. The model proposes attacks; a deterministic canary check verifies whether the synthetic secret leaked. Findings show the file or page, prompt or check, evidence, relevant framework mapping, and a suggested fix. A prepared mitigation can be applied and the same attack rerun.

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
- Return one real Playwright prompt attempt and response, one deterministic canary evaluation, and at least one independent code-scan finding with file/line evidence.

**Gate:** if hosted Playwright is not working by the end of day 2, try running the scanner and sample app in the same Docker service and verify again. If it still fails, choose another Docker host before adding features. Do not build graph polish before this path works.

### Days 3–5 — agentic attack and findings

- Add the LLM-backed attack loop: generate a prompt-injection attempt, observe the response through Playwright, and choose a bounded follow-up based on that response.
- Limit the attack to a few attempts against the bundled AI route, with request/time budgets and a synthetic canary. The deterministic evaluator alone marks a confirmed leak.
- Add a small deterministic code-scan roster for common source issues and a safe browser observation. Code checks run independently of model availability.
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

Must ship: public demo URL, bundled sample and source, one real hosted Playwright run, a conventional code scan with evidence-backed findings, graph with visible agents, and a working agentic attack/retest example.

Cut first if time runs short: voice commands, ElevenLabs narration, extra vulnerability checks, complex graph edges, custom uploads, multiple agents, and provider sign-in. Keep the one bounded LLM attack agent because it is the centerpiece. Keep animations brief and respect reduced-motion settings.

## Scope limits

- No complete OWASP Top 10 coverage claim; distinguish LLM application mappings for prompt injection from web application mappings for conventional checks, and mark uncovered categories.
- No arbitrary public target scanning, unrestricted crawling, destructive tests, or unauthorized use.
- No multi-tenant accounts, arbitrary repository uploads, multiple LLM providers, RAG features, live threat feeds, or automatic code edits.
- The LLM proposes attacks and adapts to responses; it does not decide whether an attack succeeded. A deterministic canary match confirms the finding.
- Store the LLM credential only server-side as a Cloud Run secret, enforce a strict usage budget, and disable the agentic attack clearly when no key is configured. The public judge flow must use the project-configured key, not ask judges to bring one.

## Acceptance demo

From the submission URL, a judge starts the bundled scan without setup or credentials. They see independent source-code findings and the agent adapt a prompt based on a Playwright-observed response, inspect the evidence and deterministic canary verdict, follow both crews in the graph, apply the prepared mitigation, and rerun the attack to see whether the leak persists. The core path works without voice and takes about three minutes.
