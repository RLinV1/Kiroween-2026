# Kage Crew MVP (two weeks)

## Product goal

Kage Crew is a web security demo where a crew of agents scans code and uses Playwright to test a running website. A graph shows the files and pages they inspect, their progress, and where findings appear. The judge can open evidence and rerun a check after a fix.

The MVP is a bounded sweep informed by the OWASP Top 10 for web applications. It implements a small, explicit set of checks and maps only those checks to relevant categories; it does not claim complete OWASP coverage or guarantee security.

## Demo design

The submission URL opens a working hosted demo. The judge clicks **Start demo scan** and sees two agents check a bundled, intentionally vulnerable sample: one scans its source code and one uses Playwright on its running website. Findings show the file or page, what the agent did, observed evidence, a relevant OWASP category, and a suggested fix. A prepared fix can be applied and retested so the judge sees the result change.

The hosted demo scans only this bundled target. It requires no judge API key, account, code upload, or access to the judge's computer. Keep local development supported. Do not allow public visitors to scan arbitrary websites.

## Two-week plan

### Days 1–2 — prove the hosted browser path

- Select the deployment target and deploy a minimal health-check app immediately.
- Bundle a small intentionally vulnerable website and its source code.
- Run Playwright against the deployed sample from the hosted scan service; verify the browser installs and runs in that environment.
- Return one real browser observation and one real code finding with evidence.

**Gate:** if hosted Playwright is not working by the end of day 2, switch to a host that supports a bundled browser or run the scanner in the same container as the demo app. Do not build graph polish before this path works.

### Days 3–5 — scan and findings

- Add a very small deterministic roster: for example, exposed test secret and unsafe output handling in code, plus one or two safe browser checks against the sample.
- Bound page navigation, scan duration, and requests; keep all browser activity on the bundled same-origin target.
- Show findings with file/page, action, observed evidence, category mapping, and suggested fix.

### Days 6–8 — graph crew

- Render the sample's source files and observed website pages as graph nodes.
- Show code and Playwright agents moving across the nodes they actually inspect.
- Highlight findings on their source or page node and show progress/error states.

### Days 9–10 — retest and experience

- Add a prepared sample fix and a per-finding retest that demonstrates a changed result.
- Add basic visual polish, clear empty/error states, and a simple landing/start-scan path.
- Add ElevenLabs narration if credentials and integration are ready; keep text status and controls complete without audio. Voice commands are stretch.

### Days 11–14 — stabilize and submit

- Test the complete flow from the public URL in a clean browser and on another device/network.
- Fix deployment, browser, timeout, and first-run issues; keep a short recorded walkthrough as backup.
- Finish setup instructions, demo script, and category coverage notes.

## Must-have / cutline

Must ship: public demo URL, bundled sample and source, one real hosted Playwright run, at least one real code check, graph with visible agents, evidence-backed findings, and a working retest example.

Cut first if time runs short: voice commands, ElevenLabs narration, extra vulnerability checks, complex graph edges, custom uploads, AI-generated attacks, and provider sign-in. ElevenLabs narration is the first stretch item after the core scan loop works.

## Scope limits

- No complete OWASP Top 10 coverage claim; show which categories the implemented checks cover and mark others as not covered.
- No arbitrary public target scanning, unrestricted crawling, destructive tests, or unauthorized use.
- No multi-tenant accounts, arbitrary repository uploads, multiple LLM providers, RAG features, live threat feeds, or automatic code edits.
- AI can explain findings, but initial findings must be based on observable evidence.

## Acceptance demo

From the submission URL, a judge starts the bundled scan without setup or credentials. They see source and browser agents scan the sample, follow their activity in the graph, inspect evidence and OWASP mapping, apply the prepared fix, and rerun the check to see its result change. The core path works with voice disabled and takes about three minutes.
