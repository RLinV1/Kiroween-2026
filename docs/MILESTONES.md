# Kage Crew MVP (one month)

## Product goal

Deliver a judge-ready, deployed security workbench where a bounded coordinator dispatches Code Scout, Web Attack, and RAG Attack ghost crews, then calls in the Ghostbusters to verify and retest. A public GitHub URL feeds read-only source checks; a separate hosted lab receives active SQLi, XSS, and AI/RAG attacks. The graph follows real crew events and evidence. The demo does not execute submitted repositories or attack arbitrary websites.

Map findings to relevant OWASP categories and mark unsupported coverage clearly. Display all ten OWASP LLM risks, but label unimplemented risks **Not tested in this demo**. See [OWASP mapping](OWASP_MAPPING.md).

## Four-week plan

| Week | Outcome |
|---|---|
| **1 — live foundation** | Deploy the lab and Playwright runner to Cloud Run. Accept bounded public GitHub snapshots without executing code. Prove one real code finding and one real hosted browser check. |
| **2 — crews and coordinator** | Implement the fixed workflow graph: Code Scout, Web Attack, and RAG Attack crews, plus Ghostbusters for verification. Return progress, partial results, and explicit unsupported/error states. |
| **3 — RAG and visible proof** | Add one-document Markdown/text ingestion, actual retrieval, bounded adaptive prompt injection, canary verification, SQLi/XSS proof, graph evidence links, prepared lab fixes, and per-finding retests. |
| **4 — judge-ready finish** | Stabilize Cloud Run, cap resource/model spend, polish graph and ghost/Ghostbuster feedback, verify reduced-motion/status states, and prepare the three-minute demo walkthrough. Add ElevenLabs narration only if the core path is solid. |

**End-of-month success:** a judge pastes a public repository URL, sees source findings, watches the attack crews test the separate live lab, inspects evidence, applies a prepared fix, and sees the verified retest reflected in the graph—all without local setup or personal credentials.

## Demo flow

1. Enter a public GitHub URL and start the scan.
2. The coordinator dispatches Code Scout for a bounded read-only snapshot. In parallel, Web Attack tests the lab's SQLi and XSS routes, while RAG Attack tests its retrieval chat.
3. The judge can ingest one small `.txt` or `.md` document into the RAG lab to demonstrate live poisoning/retrieval behavior.
4. Crews return evidence. Ghostbusters mark each result confirmed, not reproduced, unsupported, or errored; the LLM never decides whether its own attack succeeded.
5. Ghosts move along observed graph edges. Confirmed issues haunt their nodes; findings show target, check, evidence, OWASP mapping, and suggested fix.
6. The judge applies a prepared lab fix and retests. Ghostbusters capture a finding ghost only when the corresponding check confirms resolution.

The GitHub repository and hosted lab are separate targets, and the interface must say so. A GitHub link does not imply the submitted application was run or dynamically attacked.

## Deployment and safety

- Deploy one version-pinned Docker service to Google Cloud Run with UI/API, lab, and matching Playwright Chromium dependencies.
- Keep health/status endpoints, one bounded scan at a time, request/archive/file/runtime limits, and visible failure states.
- Accept public GitHub repositories only. Never execute code, install packages, follow repo-provided links, or show full secret values.
- Allow active browser attacks only against the hosted lab. Use synthetic data and non-destructive SQLi/XSS proof cases.
- Store the provider credential server-side as a Cloud Run secret; cap calls and clearly mark the RAG Attack crew unavailable if the key is missing.
- Keep a local Docker flow for development. A CLI for users' local apps and arbitrary live-site attacks are future work.

## Must-have / cutline

Must ship: public demo URL, public-repo code scan with evidence, hosted lab with at least one verified SQLi or XSS finding, real RAG retrieval plus one bounded prompt-injection/leakage run, coordinator showing distinct crew activity, graph/evidence view, and a working Ghostbusters fix/retest.

Cut first if time runs short: voice, broad source-language coverage, extra attack variations, elaborate graph edges, private repos, arbitrary targets, and local CLI. Keep worker roles visible, the evidence verifier, and one end-to-end fix/retest path.

## Scope limits

- No claim of full OWASP Top 10 coverage or a general-purpose penetration test.
- No arbitrary live-site scans, unrestricted crawling, destructive tests, or unauthorized use.
- No execution of submitted GitHub code, private-repository support, or automatic source edits.
- No multi-tenant accounts, multiple providers, runtime MCP discovery, persistent history, or live threat feeds.
- The coordinator dispatches only fixed worker types with bounded inputs and budgets; worker/model claims are not findings without verifier evidence.
