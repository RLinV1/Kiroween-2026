# Kage Crew MVP brainstorming

## Intent

Build a judge-ready web demo of Kage Crew in two weeks, centered on an agentic prompt-injection attack. An LLM agent adapts a bounded sequence of attempts to a bundled AI website through Playwright. The graph makes its path and findings visible; code checks support the wider security story. ElevenLabs narration follows only after the attack loop works.

## Name

“Kage” (影) means “shadow” in Japanese, suggesting the ghost agents that quietly trace a codebase and probe a site's behavior. “Crew” emphasizes that the scanners work as a coordinated team. The name keeps the compact, agent-team feel that inspired it while giving this project its own identity.

## Approaches considered

1. **Prompt-injection lab only.** Fastest to build, but lacks the requested code-scan and graph crew context.
2. **Autonomous scanner for arbitrary websites.** Closest to the long-term vision, but safe crawling, reliable findings, false-positive control, and broad coverage are too much to deliver credibly in two weeks.
3. **Agentic prompt-injection centerpiece plus supporting code checks, Playwright, hosted sample, and graph. Recommended.** Keeps the signature AI-agent behavior in front while bounding the target and supporting checks enough to finish a reliable demo URL in two weeks.

## Goals

- Publish a working demo URL with a bundled sample project and intentionally vulnerable site.
- Make one bounded LLM-backed prompt-injection agent the centerpiece; it adapts a follow-up based on Playwright-observed responses.
- Use a deterministic canary match to verify success; do not let the LLM declare its own attack successful.
- Map prompt injection to applicable OWASP Top 10 for LLM Applications guidance; map conventional checks to the OWASP Top 10 for Web Applications only where relevant.
- Include a small set of evidence-based code and browser checks.
- Restrict Playwright to the bundled AI target and safe same-origin actions.
- Show files, observed pages, checks, and findings in a graph; do not imply inferred relationships are proven data flow.
- Use a few clear animations to show scanning, findings, the blue-team capture, and retest outcomes.
- Present evidence, relevant OWASP mapping, suggested fixes, and individual retests.
- Add optional ElevenLabs narration after the hosted attack loop works; defer speech recognition and voice commands.
- Let judges experience the demo without API credentials, repository uploads, or broad permissions.

## Non-goals

- Full OWASP Top 10 coverage, comprehensive penetration testing, or guaranteed discovery.
- Unbounded crawling, destructive actions, arbitrary public target scanning, or automatic code changes.
- Multi-tenant SaaS, many provider integrations, RAG features, persistent agent memory, and live threat feeds.

## Proposed run

The judge opens the deployed app and starts the bundled attack. A single LLM agent proposes a prompt injection, Playwright submits it to the paired AI chat page, and the agent selects a bounded follow-up based on the observed response. A deterministic canary check decides whether the target leaked its synthetic secret. A code ghost also runs a small deterministic roster over the sample source. Both streams populate a graph of observed files and pages. Findings include source/page, prompt or check, observed proof, applicable OWASP category, and remediation guidance. The judge applies the prepared prompt-defense fix and reruns the attack. ElevenLabs narration is optional; the UI works without it.

Ghosts emerge at scan start and travel to the nodes being checked. A finding pulses red and opens its evidence. The blue-team capture beam contains that ghost and turns the node blue. On retest, a shield holds if the issue no longer reproduces; otherwise, the ghost breaks through. Keep motion brief and respect reduced-motion preferences.

## Success and constraints

The complete demo should take about three minutes and work from the submitted URL without judge credentials or setup. Deployment and a real hosted Playwright run are proved in the first two days, before graph polish. Development and testing remain possible locally. The hosted environment scans only the bundled target; arbitrary website scanning is outside this MVP.

The month plan is: weeks one and two deliver the complete deployed attack/evidence/defense/retest loop; weeks three and four stabilize that experience, add optional narration and polish where time allows, then prepare the submission. Success is a reliable judge demo, not a larger attack catalog.

## Builder decisions

- Choose one stable model provider and a hard limit of a few attack attempts; use a small reliable roster of code/browser checks as supporting scope.
- Use deterministic evidence checks for whether a finding exists; use AI to explain or prioritize rather than fabricate proof.
- Select hosting that supports the app and Playwright browser service within the available time and budget.
- Configure the model credential server-side for the public demo, cap usage, and do not ask judges for keys. If it is unavailable, explain that the agentic attack is offline rather than presenting scripted prompts as an autonomous attack.
- Keep ElevenLabs optional so it cannot prevent judges from seeing core scan results.
