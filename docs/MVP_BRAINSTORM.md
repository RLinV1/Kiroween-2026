# Kage Crew MVP brainstorming

## Intent

Build a judge-ready web demo of Kage Crew in two weeks: a crew of security agents that scan code and use Playwright to test a running site. The graph makes the agents' paths and findings visible; ElevenLabs can give the crew a voice after the working scan loop is proven. The product should show how static code checks and live browser behavior complement each other.

## Name

“Kage” (影) means “shadow” in Japanese, suggesting the ghost agents that quietly trace a codebase and probe a site's behavior. “Crew” emphasizes that the scanners work as a coordinated team. The name keeps the compact, agent-team feel that inspired it while giving this project its own identity.

## Approaches considered

1. **Prompt-injection lab only.** Fast to build, but too narrow for a regular website security sweep and does not demonstrate code scanning alongside browser testing.
2. **Autonomous scanner for arbitrary websites.** Closest to the long-term vision, but safe crawling, reliable findings, false-positive control, and broad coverage are too much to deliver credibly in a month.
3. **Bounded code and Playwright sweep with a hosted sample, graph, and voice. Recommended.** Preserves the product's distinctive experience while limiting checks and targets enough to finish a stable demo that judges can access by URL.

## Goals

- Publish a working demo URL with a bundled sample project and intentionally vulnerable site.
- Run a small set of evidence-based code and browser checks informed by the OWASP Top 10 for web applications.
- Use Playwright for bounded, same-origin navigation and safe interactions.
- Show files, observed pages, checks, and findings in a graph; do not imply inferred relationships are proven data flow.
- Use a few clear animations to show scanning, findings, the blue-team capture, and retest outcomes.
- Present evidence, relevant OWASP mapping, suggested fixes, and individual retests.
- Use ElevenLabs for narration and a small voice-command set, with typed controls available throughout.
- Let judges experience the demo without API credentials, repository uploads, or broad permissions.

## Non-goals

- Full OWASP Top 10 coverage, comprehensive penetration testing, or guaranteed discovery.
- Unbounded crawling, destructive actions, arbitrary public target scanning, or automatic code changes.
- Multi-tenant SaaS, many provider integrations, RAG features, persistent agent memory, and live threat feeds.

## Proposed run

The judge opens the deployed app and starts the bundled sample scan. Code ghosts run deterministic checks against the sample project; browser ghosts use Playwright on its paired demo website. Both streams populate a graph of observed files and pages. Findings include source/page, check, action, observed proof, applicable OWASP category, and remediation guidance. The judge applies a safe sample fix and reruns that check. ElevenLabs narrates progress when configured; the UI remains fully operable when voice is unavailable.

Ghosts emerge at scan start and travel to the nodes being checked. A finding pulses red and opens its evidence. The blue-team capture beam contains that ghost and turns the node blue. On retest, a shield holds if the issue no longer reproduces; otherwise, the ghost breaks through. Keep motion brief and respect reduced-motion preferences.

## Success and constraints

The complete demo should take about three minutes and work from the submitted URL without judge credentials or setup. Deployment and a real hosted Playwright run are proved in the first two days, before graph polish. Development and testing remain possible locally. The hosted environment scans only the bundled target; arbitrary website scanning is outside this MVP.

## Builder decisions

- Choose a minimal, reliable roster of code and browser checks and document each check's coverage.
- Use deterministic evidence checks for whether a finding exists; use AI to explain or prioritize rather than fabricate proof.
- Select hosting that supports the app and Playwright browser service within the available time and budget.
- Confirm current eligibility and constraints before depending on Sign in with ChatGPT or ElevenLabs; neither should prevent judges from seeing core scan results.
