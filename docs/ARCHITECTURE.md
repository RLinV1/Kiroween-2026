# Kage Crew Architecture (MVP)

## Product shape

Kage Crew is a graph-based security workbench with two distinct targets: a public GitHub repository for read-only source checks, and a hosted, intentionally vulnerable lab for active browser attacks. A bounded coordinator runs specialist workers, combines their evidence, and drives the ghost graph. A public repository is never executed, and the active attack crew only targets the Kage Crew lab in the MVP.

## Orchestrated scan

The coordinator creates a bounded workflow graph and dispatches independent workers:

1. **Repo Scout** validates and fetches a public GitHub repository snapshot, inventories supported languages and manifests, and reports unsupported areas. It does not execute repository code.
2. **Code Security worker** runs deterministic secret, dependency, and common source checks over that snapshot. Findings include file and line evidence where available; secret values are redacted.
3. **Web Attack crew** uses Playwright and safe deterministic checks against the lab's vulnerable SQL and XSS routes.
4. **AI/RAG Red Team agent** attacks the lab chat with bounded, response-informed prompt injections. The judge can ingest one Markdown or text document into the lab's real retrieval path to demonstrate poisoning and leakage.
5. **Evidence Verifier** checks each reported finding using deterministic proof rules. The language model can propose and adapt prompts, but cannot declare its own success.

The code scan and lab attacks may run in parallel. The coordinator tracks status, limits, errors, and dependencies; it does not let workers choose arbitrary targets or spawn unbounded agents. Findings flow into one report and graph. Retest dispatches the relevant check again after the judge applies a prepared lab fix.

```mermaid
flowchart LR
    U[GitHub URL + scan request] --> O[Coordinator]
    O --> R[Repo Scout]
    R --> C[Code Security]
    O --> W[Playwright Web Attack]
    O --> A[AI/RAG Red Team]
    C --> V[Evidence Verifier]
    W --> V
    A --> V
    V --> G[Graph + findings]
    G --> F[Prepared fix + retest]
    F --> O
```

## Run flow

1. Judge pastes a public GitHub URL and starts a scan.
2. Repo Scout and Code Security scan the repository snapshot; Web Attack and AI/RAG Red Team test the hosted lab. The UI labels these as separate targets.
3. Specialist workers emit progress and evidence nodes. Ghosts move only along workflow and target relationships observed by the run.
4. Evidence Verifier records confirmed, not reproduced, unsupported, or error states. Confirmed findings show source/page, check, observed evidence, and relevant OWASP mapping.
5. Judge applies a prepared fix in the lab and retests. Ghostbusters capture a finding ghost only when the corresponding verifier confirms the issue no longer reproduces; otherwise it remains.
6. ElevenLabs narration may announce progress and findings if configured. Typed controls and visible status remain available without voice.

## Safety and limits

- GitHub input accepts public repositories only, with archive size, file count, and runtime limits. Never execute repository code, install its dependencies, or expose secret values in output.
- Active attacks target only the bundled lab in the public MVP. The submitted repository is not treated as a live app.
- SQLi and XSS checks use non-destructive test cases and synthetic data. RAG documents and canaries are isolated to the lab.
- Bound prompt count, response size, runtime, browser navigation, scan concurrency, and model usage.
- Show unsupported languages and unavailable checks explicitly; accepting a repository URL does not claim complete coverage.
- Map prompt injection and confirmed synthetic data disclosure to the relevant OWASP LLM categories. Map conventional findings to applicable OWASP Web Application categories; do not claim full coverage.

## Hosted deployment

Use a version-pinned Docker service on Google Cloud Run for the UI/API, security lab, and Playwright Chromium runtime. Add a health endpoint, one-at-a-time scan control, request and archive limits, and visible errors. Keep a local Docker run path for the same app and lab.

## MVP components

- **Coordinator:** bounded graph workflow that dispatches workers, aggregates events, and starts evidence-backed retests.
- **Repo intake and code checks:** read-only public GitHub snapshot and deterministic secret, dependency, and common source checks.
- **Web Attack crew:** Playwright tests for SQLi and XSS against the fixed lab routes.
- **AI/RAG Red Team:** one model-backed specialist that performs bounded, adaptive prompt-injection attempts against the lab's retrieval chat.
- **Evidence Verifier:** deterministic checks for source matches, harmless browser effects, and synthetic canary exposure.
- **Lab:** deliberately vulnerable web routes plus a RAG chat, isolated synthetic data, one-document Markdown/text ingestion, and prepared remediations.
- **Graph:** coordinator, workers, repository files, lab pages, retrieved chunks, findings, and evidence appear as nodes connected by observed run events.
- **Voice:** optional ElevenLabs TTS; STT and voice commands are deferred.

## Model credentials

Use one developer-configured server-side provider credential stored as a Cloud Run secret. Enforce strict request and token budgets; never expose or log the key. Deterministic source and browser checks remain usable if model access is unavailable. Mark the AI/RAG worker unavailable rather than substituting scripted prompts and claiming autonomy.

## Deferred

Arbitrary live website attacks, private repository access, execution of submitted code, local development CLI, broad language coverage, unrestricted crawling, multi-tenant accounts, multiple model providers, MCP discovery, automatic source edits, persistent history, and live threat intelligence.
