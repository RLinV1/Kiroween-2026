# Kage Crew MVP Design

## Experience

The judge opens the Cloud Run URL, enters a public GitHub repository, and starts a scan. The graph shows the coordinator dispatching code-analysis workers and attack crews to their separate targets: the submitted repository is inspected read-only, while active Playwright tests run only against Kage Crew's hosted lab. The judge can ingest one text/Markdown document into the lab's RAG store and watch the AI/RAG agent test it.

Every finding opens its source location or lab route, the check and action, observed response/retrieved chunk, deterministic proof, and suggested mitigation. After the judge applies a prepared lab fix, the coordinator reruns the matching worker. The Ghostbuster beam captures the attack ghost only when the verifier confirms the finding is gone; otherwise the ghost stays in the node. ElevenLabs narration is optional.

## Orchestration architecture

Implement the coordinator as a bounded workflow graph rather than a free-running swarm. It owns a scan ID, target limits, worker status, event stream, and evidence aggregation. The source lane and lab lane can run independently; Evidence Verifier runs when each worker returns.

| Worker | Responsibility | Success evidence |
|---|---|---|
| **Repo Scout** | Validate a public GitHub URL, fetch a bounded snapshot, inventory manifests/languages, mark unsupported areas. Never execute source. | Snapshot identity, supported file inventory, unsupported coverage. |
| **Code Security** | Run deterministic secret, dependency, and common source checks. | Rule ID and file/line evidence; secret values redacted. |
| **Web Attack crew** | Use Playwright to test the lab's SQLi and XSS routes with non-destructive probes. | Route, action, observed response, and harmless proof marker. |
| **AI/RAG Red Team** | Use an LLM to propose and adapt bounded prompt-injection attempts against the lab retrieval chat. | Prompt/response trace and actual retrieved chunks. |
| **Evidence Verifier** | Apply deterministic per-check proof rules and assign confirmed, not reproduced, unsupported, or error status. | Reproducible proof; synthetic canary match for data leakage. |

The coordinator dispatches fixed worker types only. Workers cannot add new workers, change the active target, or bypass time/request limits. A worker failure becomes visible without hiding results from other workers.

## Lab and graph

- **Lab:** small intentionally vulnerable web app with isolated SQLi/XSS routes and an actual RAG chat pipeline. Permit one bounded `.txt` or `.md` document ingestion per demo run. The target has a synthetic restricted canary and prepared fixes.
- **Graph nodes:** coordinator, worker, repository file/manifest, lab route, retrieved chunk, finding, and evidence.
- **Graph edges:** dispatch, inspected, navigated, retrieved, produced finding, and retested. Add edges only when the run records the relation.
- **Animation:** red ghosts move to tested nodes; a confirmed issue haunts the affected node. Ghostbusters capture it only after a verified successful retest. An unresolved finding remains visible. Error and unsupported states are distinct from passes.

## Service layout

- **Web app:** React/Vite/TypeScript interface for repo input, scan status, graph, evidence, and retest.
- **API/coordinator:** FastAPI validates input, creates the bounded workflow, reports progress, and aggregates evidence.
- **Repo intake:** fetches public repository snapshots with archive/file limits; never invokes build scripts or package managers.
- **Workers:** deterministic source checks and browser packs, plus one bounded LLM-backed AI/RAG attack worker.
- **Browser runner:** version-pinned Playwright Chromium. Active navigation is allowlisted to the hosted lab origin.
- **Evaluator:** deterministic check-specific rules; model output alone never creates a confirmed finding.

## Deployment

Build a version-pinned Docker image with the web app, API/coordinator, lab, and Playwright browser dependencies. Deploy to Google Cloud Run with health checks, low concurrency, one bounded scan at a time, and archive/request/runtime caps. Keep the provider key in a Cloud Run secret. Maintain a local Docker flow for development and the same bundled lab.

## Failure and safety behavior

Reject private repositories and invalid/oversized GitHub inputs. Do not execute repo code or install dependencies. Reject browser navigation outside the lab origin. If model access is unavailable, run source and conventional lab checks and mark the AI/RAG worker unavailable. Show timeouts, unsupported languages, partial coverage, and worker errors explicitly; none counts as a clean pass.

## Deliberately deferred

Arbitrary live-site targets, local development CLI, private repository integration, broad language support, unrestricted crawling, general autonomous penetration testing, multiple model providers, automatic source edits, persistent history, MCP discovery, STT, and multi-tenant hosting.
