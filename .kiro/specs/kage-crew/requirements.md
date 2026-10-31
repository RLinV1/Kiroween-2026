# Kage Crew MVP Requirements

## Goal

Kage Crew is a judge-accessible security workbench with a graph-based multi-agent orchestrator. It scans a public GitHub repository using read-only code checks and runs bounded active tests against a separate hosted lab. Specialist workers test conventional web vulnerabilities and an AI/RAG chat; a deterministic verifier grounds every confirmed finding in evidence.

## In scope

1. **Hosted demo:** public Google Cloud Run URL; a judge can run a scan without an account, personal API key, or local setup.
2. **Public repository input:** accept a public GitHub repository URL, fetch a bounded snapshot, identify supported languages, and never execute the repository or install its dependencies.
3. **Code Security worker:** run deterministic secret, dependency, and common source checks. Show file/line evidence where available, redact secret values, and label unsupported checks/languages.
4. **Hosted security lab:** include intentionally vulnerable SQLi and XSS routes plus a RAG chat backed by actual document retrieval. The judge can ingest one `.txt` or `.md` document during a run.
5. **Coordinator:** dispatch the Repo Scout, Code Security worker, Playwright Web Attack crew, AI/RAG Red Team, and Evidence Verifier through a bounded workflow graph. Track progress, limits, errors, and findings. Workers cannot select arbitrary targets or spawn more workers.
6. **Web Attack crew:** use Playwright for non-destructive SQLi and XSS checks against only the hosted lab; capture route, action, response, and proof.
7. **AI/RAG Red Team:** use one LLM-backed specialist to attempt prompt injection against the hosted RAG chat and adapt a bounded follow-up to observed responses. Enforce attempt, time, and model-usage budgets.
8. **Evidence Verifier:** use deterministic rules to confirm or reject findings, including synthetic canary exposure. The LLM must not grade its own success.
9. **Graph and ghosts:** show workflow workers, repository files, lab routes, retrieved chunks, findings, and evidence as nodes/observed edges. Animate attacker ghosts on confirmed vulnerable nodes and connect each result to its evidence.
10. **Blue-team retest:** allow a prepared lab fix and rerun the relevant check. Ghostbusters capture the finding ghost only when verification confirms the issue no longer reproduces; otherwise it remains.
11. **OWASP coverage:** show all ten OWASP LLM risks as a coverage map, marking actual tested categories and labeling the rest **Not tested in this demo**. Map web findings only to relevant categories. Do not claim full coverage.
12. **Voice stretch:** ElevenLabs narration may announce progress/findings. Typed controls and text status remain complete without voice.

## Public-demo safety

- Active browser attacks target only the hosted lab; the GitHub repository is a separate static-analysis input.
- Accept public repositories only, with archive size, file count, and scan time limits. Do not execute repository code, install dependencies, follow arbitrary links, or reveal full secret values.
- Use isolated synthetic records, a synthetic canary, and non-destructive SQLi/XSS checks. Limit lab document ingestion to small text/Markdown files.
- Bound browser navigation, scan concurrency, request sizes, attempts, model calls, and runtime.
- Store the model key only as a server-side Cloud Run secret. If absent, show the AI/RAG worker as unavailable; do not replace it with scripted behavior and call it autonomous.

## Acceptance criteria

- The deployed app accepts a public GitHub URL and clearly reports completed, partial, unsupported, or failed checks.
- Source findings include file/line evidence where available, and secret values are redacted.
- In one run, the coordinator reports progress from independent code and lab workers and gathers their evidence in one graph/report.
- Playwright reproduces at least one conventional lab issue and the AI/RAG worker performs a response-informed attack against the real retrieval chat.
- Deterministic verification controls confirmed status; retrieved chunks, browser responses, and source locations support the result.
- A prepared lab fix changes a verified result on retest. The ghost is captured only after the fix is verified.
- The graph represents actual run events and supports readable text status and reduced-motion settings.
- The judge-facing path needs no personal credentials or local installation and completes in about three minutes.

## Out of scope

Arbitrary live-site attacks, private repositories, executing submitted code, a local development CLI, unrestricted crawling, comprehensive OWASP coverage, multi-tenant accounts, multiple model providers, automatic source edits, persistent history, runtime MCP discovery, and live threat intelligence.
