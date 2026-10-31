# Kage Crew MVP (one month)

## Product goal

Kage Crew is a web security workbench built around a crew of AI-assisted security agents. It scans a supplied code project and uses Playwright to exercise its running website. A graph shows the project and pages the crew has observed, the checks moving through them, and where findings were detected. ElevenLabs provides voice commands and narration.

The MVP is a bounded security sweep informed by the OWASP Top 10 for web applications. It will implement a small, explicit set of checks and map each one to the relevant OWASP category. It will not claim complete coverage of all ten categories or guarantee that a target is secure.

## MVP scope

- **Working demo URL:** deploy the frontend and scan service, with a bundled sample project and intentionally vulnerable demo website that judges can scan without uploading their own code. Keep development and local testing supported.
- **Code scan:** accept a project archive or connect to the bundled sample. Run a small deterministic roster for clear issues such as exposed secrets and selected unsafe patterns. Show file and line evidence when available.
- **Playwright scan:** exercise the bundled demo site, navigate a bounded set of same-origin pages, inspect forms, and run safe checks. Show the URL, action, and observed evidence for every result. No destructive actions or unrestricted crawling.
- **OWASP mapping:** use the OWASP Top 10 for web applications as the organizing framework. Include only categories the implemented checks actually examine; label the rest as not covered in this MVP.
- **Agent crew:** provide distinct code and browser agents with bounded tasks, request budgets, and timeouts. AI can help prioritize or explain results, but the initial finding roster must be based on observable evidence.
- **Graph:** show project files/modules and pages/forms as nodes. Add edges only for relationships observed by the scanner. Animate each ghost along the nodes it checks and highlight evidence on the affected node.
- **Findings and retest:** show severity, OWASP category, evidence, explanation, and remediation guidance. Let the user rerun a check and compare its result after a fix; report observations without promising complete remediation.
- **Voice:** ElevenLabs handles a small command set and reads progress/findings aloud. Typed controls remain available. The deployed demo should degrade clearly if voice credentials or service access are unavailable.
- **LLM connection:** keep model access to one supported path for the demo. Evaluate Sign in with ChatGPT plan usage for local/open-source use; do not make it a prerequisite for a judge to try the bundled deterministic checks.
- **Safe hosted target:** the public demo scans only its bundled, intentionally vulnerable target. Arbitrary URL scanning is limited to local development or explicitly authorized targets, with clear scope and request limits.

## Four-week sequence

### Week 1 — scan spine

Define the supported project input and first check roster. Build the bundled vulnerable website, run one code check and one Playwright check, and return evidence in a basic results view.

### Week 2 — bounded sweep and graph

Add the selected checks, safe same-origin navigation, OWASP mapping, and graph nodes for files and observed pages. Show scan progress and attach findings to graph nodes.

### Week 3 — agent crew and voice

Give checks distinct ghost identities and animate them through the graph. Add ElevenLabs narration and a small set of voice commands, with typed controls and audio-off support.

### Week 4 — retest and deploy

Add per-finding reruns, before/after status, common error handling, deployment of the judge-ready demo, setup documentation, and a short demo path.

## Deferred

- Complete OWASP Top 10 coverage or a claim of general-purpose security auditing.
- Unrestricted autonomous attack generation, arbitrary target crawling, destructive testing, or scanning without authorization.
- Broad support for uploads of arbitrary repositories, private repository integrations, production multi-tenant accounts, and durable user history.
- Multiple LLM vendors, RAG uploads/poisoning, live threat feeds, persistent agent memory, and exhaustive vulnerability catalogs.
- Automatic code changes or claims that passing retests prove a site is secure.

## Acceptance demo

From the submitted demo URL, a judge starts a scan of the bundled sample. They see code and Playwright ghosts traverse the graph, open findings with file/page evidence and relevant OWASP categories, hear ElevenLabs narrate progress, then rerun a check after applying a sample fix and compare the result. The core demo remains usable without voice and takes about five minutes.
