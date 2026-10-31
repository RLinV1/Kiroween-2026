# Kage Crew MVP Requirements

## Goal

Kage Crew is a judge-accessible, graph-based security demo. Its centerpiece is a bounded, LLM-backed prompt-injection agent that attacks a bundled AI chat website through Playwright, adapts to observed responses, and presents verifiable evidence. Small code and browser checks support a regular security-sweep story.

## In scope

1. **Hosted demo:** public URL on Google Cloud Run; a judge can start the bundled scan without an account, code upload, or personal API key.
2. **Bundled target:** intentionally vulnerable AI chat page and its source, including a synthetic canary secret and a prepared mitigation toggle.
3. **Agentic attack:** one model-backed agent proposes a prompt-injection attempt, receives the target's observed response, and may choose a bounded follow-up. Enforce a small attempt cap, time limit, and model-usage budget.
4. **Playwright:** drive the bundled same-origin target UI and capture the submitted prompt, page, response, and run status.
5. **Verdict:** a deterministic matcher checks for the synthetic canary. The LLM must not decide whether it succeeded. A finding includes the evidence that triggered the verdict.
6. **Code checks:** a small deterministic roster over the bundled target source, with file and line evidence when available.
7. **Graph and ghosts:** show observed files/pages and checks as nodes; animate agents along observed relationships; connect findings to evidence nodes.
8. **Blue-team demo:** apply the prepared prompt-defense change, capture the attacking ghost, and rerun the same bounded attack. Report whether the canary still appears.
9. **OWASP mapping:** map prompt injection to applicable OWASP Top 10 for LLM Applications guidance and conventional findings to OWASP Top 10 for Web Applications categories where relevant. Mark unsupported categories not covered.
10. **Voice stretch:** ElevenLabs narration may announce progress/findings. Typed controls and visible text status are required even when voice is unavailable.

## Public-demo safety

- Public scans target only the bundled sample. Do not accept arbitrary public URLs or uploaded projects in the MVP.
- Keep the canary synthetic; do not use real secrets or personal data.
- Store the developer-configured model key as a server-side Cloud Run secret; never expose or log it. If absent, disable agentic generation clearly while keeping deterministic scans available.
- Bound browser navigation, scan concurrency, attempts, model calls, request size, and runtime.

## Acceptance criteria

- The deployed URL can start and complete a scan from a clean browser without judge setup.
- At least one real Playwright prompt/response and a real bundled-source finding appear with evidence.
- The attack agent makes a response-informed follow-up within its limit; canary detection, not model self-report, determines success.
- A prepared mitigation and retest produce an observable changed outcome.
- The graph shows scan progress, findings, capture, and retest state. Motion respects reduced-motion settings.
- The core path takes about three minutes and does not require ElevenLabs.

## Out of scope

Arbitrary target scanning, general-purpose autonomous penetration testing, full OWASP Top 10 coverage, multi-tenant accounts, multiple model vendors, RAG, runtime MCP tool discovery, unrestricted crawling, destructive actions, automatic code changes, and persistent scan history.
