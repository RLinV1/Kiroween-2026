# Kage Crew MVP Design

## Experience

The judge opens the Cloud Run URL, enters a public GitHub repository, and starts a scan. The graph shows the coordinator dispatching the Code Scout, Web Attack, and RAG Attack crews to their separate targets: the submitted repository is inspected read-only, while active Playwright tests run only against Kage Crew's hosted lab. The judge can ingest one text/Markdown document into the lab's RAG store and watch the RAG Attack crew test it.

Every finding opens its source location or lab route, the check and action, observed response/retrieved chunk, deterministic proof, and suggested mitigation. After the judge applies a prepared lab fix, the coordinator reruns the matching worker. The Ghostbuster beam captures the attack ghost only when the verifier confirms the finding is gone; otherwise the ghost stays in the node. ElevenLabs narration is optional.

## Orchestration architecture

Implement the coordinator as a bounded workflow graph rather than a free-running swarm. It owns a scan ID, target limits, crew status, event stream, and evidence aggregation. The source lane and lab lane can run independently; Ghostbusters verify each crew's findings as they return.

| Worker | Responsibility | Success evidence |
|---|---|---|
| **Code Scout crew** | Validate a public GitHub URL, fetch a bounded snapshot, inventory manifests/languages, then run deterministic checks. Never execute source. | Snapshot identity, supported-file inventory, rule IDs, file/line evidence; secrets redacted. |
| **Web Attack crew** | Use Playwright to test the lab's SQLi and XSS routes with non-destructive probes. | Route, action, observed response, and harmless proof marker. |
| **RAG Attack crew** | Use an LLM to propose and adapt bounded prompt-injection attempts against the lab retrieval chat. | Prompt/response trace and actual retrieved chunks. |
| **Ghostbusters crew** | Apply deterministic per-check proof rules, assign confirmed/not reproduced/unsupported/error status, and retest after fixes. | Reproducible proof; synthetic canary match for data leakage. |

The coordinator dispatches only these fixed crews. Crews cannot add new crews, change their assigned target, or bypass time/request limits. A crew failure becomes visible without hiding results from other crews.

## Lab and graph

- **Lab:** small intentionally vulnerable web app with isolated SQLi/XSS routes and an actual RAG chat pipeline. Permit one bounded `.txt` or `.md` document ingestion per demo run. The target has a synthetic restricted canary and prepared fixes.
- **Graph nodes:** coordinator, ghost crew, repository file/manifest, lab route, retrieved chunk, finding, and evidence. Use a distinct visual identity for each crew.
- **Graph edges:** dispatch, inspected, navigated, retrieved, produced finding, and retested. Add edges only when the run records the relation.
- **Animation:** each attacker crew has its own ghost style/color and moves to the nodes it tests; a confirmed issue haunts the affected node. Ghostbusters capture it only after a verified successful retest. An unresolved finding remains visible. Error and unsupported states are distinct from passes.

## Service layout

- **Web app:** React/Vite/TypeScript interface for repo input, scan status, graph, evidence, and retest.
- **API/coordinator:** FastAPI validates input, creates the bounded workflow, reports progress, and aggregates evidence.
- **Repo intake:** fetches public repository snapshots with archive/file limits; never invokes build scripts or package managers.
- **Crews:** Code Scout uses deterministic source checks; Web Attack uses bounded Playwright packs; RAG Attack uses one LLM-backed adaptive agent; Ghostbusters verify and retest deterministically.
- **Browser runner:** version-pinned Playwright Chromium. Active navigation is allowlisted to the hosted lab origin.
- **Evaluator:** deterministic check-specific rules; model output alone never creates a confirmed finding.

## Deployment

Build a version-pinned Docker image with the web app, API/coordinator, lab, and Playwright browser dependencies. Deploy to Google Cloud Run with health checks, low concurrency, one bounded scan at a time, and archive/request/runtime caps. Keep the provider key in a Cloud Run secret. Maintain a local Docker flow for development and the same bundled lab.

## Failure and safety behavior

Reject private repositories and invalid/oversized GitHub inputs. Do not execute repo code or install dependencies. Reject browser navigation outside the lab origin. If model access is unavailable, run source and conventional lab checks and mark the RAG Attack crew unavailable. Show timeouts, unsupported languages, partial coverage, and crew errors explicitly; none counts as a clean pass.

## Deliberately deferred

Arbitrary live-site targets, local development CLI, private repository integration, broad language support, unrestricted crawling, general autonomous penetration testing, multiple model providers, automatic source edits, persistent history, MCP discovery, STT, and multi-tenant hosting.
