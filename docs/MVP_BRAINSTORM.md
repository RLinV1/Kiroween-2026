# Kage Crew MVP brainstorming

## Intent

Build a judge-ready Kage Crew web demo in one month. A bounded multi-agent coordinator combines a read-only public GitHub code scan with active attacks against a separate hosted security lab. The graph shows each worker's real path, evidence, and retest. ElevenLabs narration follows only after the core scan works.

## Name

“Kage” (影) means “shadow” in Japanese, suggesting the ghost agents that quietly trace a codebase and probe a site's behavior. “Crew” emphasizes that the scanners work as a coordinated team. The name keeps the compact, agent-team feel that inspired it while giving this project its own identity.

## Approaches considered

1. **AI attack lab only.** Fastest to build, but misses ordinary source and web security checks.
2. **Autonomous scanner for arbitrary live websites.** Closest to the long-term vision, but safe crawling, auth handling, and reliable findings are too much to deliver credibly in one month.
3. **Public-repo code scan plus bounded multi-agent attacks on a hosted lab. Recommended.** Gives judges real input, combines ordinary and AI security checks, and keeps active testing safe and repeatable.

## Goals

- Publish a working demo URL where judges can submit a public GitHub repository and run the hosted lab.
- Use a bounded coordinator to dispatch Code Scout, Web Attack, and RAG Attack ghost crews, then dispatch Ghostbusters to verify and retest.
- Check repositories read-only for secrets, dependencies, and common source issues; never execute them.
- Run non-destructive SQLi and XSS checks against the lab and bounded prompt-injection/RAG checks against its actual retrieval chat.
- Allow one live `.txt` or `.md` document ingestion into the lab to demonstrate poisoning and possible canary leakage.
- Use deterministic evidence to confirm results; the LLM does not grade its own attacks.
- Map confirmed AI findings to relevant OWASP LLM categories and conventional checks to relevant OWASP Web Application categories. Mark untested risks clearly.
- Show ghost crews, repository files, lab pages, retrieved chunks, and findings as graph nodes connected by observed run events.
- Use a few clear animations to show scanning, findings, the blue-team capture, and retest outcomes.
- Present evidence, relevant OWASP mapping, suggested fixes, and individual retests.
- Display all ten OWASP LLM risks in a coverage map; clearly distinguish implemented checks from risks not tested by this demo.
- Add optional ElevenLabs narration after the hosted scan works; defer speech recognition and voice commands.
- Let judges use the demo without an account or personal API key; enforce bounded repository and lab input.

## Non-goals

- Full OWASP Top 10 coverage, comprehensive penetration testing, or guaranteed discovery.
- Unbounded crawling, destructive actions, arbitrary public target scanning, or automatic code changes.
- Multi-tenant SaaS, private repositories, arbitrary live-site attacks, repository code execution, local development CLI, persistent agent memory, and live threat feeds.

## Proposed run

The judge submits a public GitHub URL and starts a scan. The coordinator dispatches the Code Scout crew and the two lab attack crews. Web Attack checks SQLi and XSS; RAG Attack tests the retrieval chat, using one document ingested during the demo. Ghostbusters verify each result and retest after a fix. The graph shows each crew, source files, lab routes, retrieved chunks, and proof. Findings include target, check, evidence, relevant OWASP mapping, and remediation. Ghostbusters capture only verified fixes. ElevenLabs narration is optional.

Ghosts emerge at scan start and travel to the nodes being checked. A finding pulses red and opens its evidence. The blue-team capture beam contains that ghost and turns the node blue. On retest, a shield holds if the issue no longer reproduces; otherwise, the ghost breaks through. Keep motion brief and respect reduced-motion preferences.

## Success and constraints

The complete demo should take about three minutes and work from the submitted URL without judge credentials or local setup. Repo analysis is read-only; active testing stays on the hosted lab. Prove Cloud Run deployment and a real hosted Playwright run early. A local Docker path supports development; a local-development CLI and arbitrary website scanning are future work.

The one-month plan is in `docs/MILESTONES.md`: first prove Cloud Run, repository intake, and Playwright; then deliver the fixed coordinator workflow; add live RAG ingestion and verified retests; finish with stability and visual polish. Success is a reliable judge demo, not a larger attack catalog.

## Builder decisions

- Keep the coordinator workflow fixed and bounded; give each ghost crew explicit inputs, outputs, and limits.
- Choose one stable model provider and a hard limit of a few AI/RAG attack attempts; keep code, SQLi, and XSS checks deterministic where possible.
- Use deterministic evidence checks for whether a finding exists; never let AI fabricate proof or grade its own attack.
- Select hosting that supports the app and Playwright browser service within the available time and budget.
- Configure the model credential server-side for the public demo, cap usage, and do not ask judges for keys. If it is unavailable, explain that the agentic attack is offline rather than presenting scripted prompts as an autonomous attack.
- Keep ElevenLabs optional so it cannot prevent judges from seeing core scan results.
