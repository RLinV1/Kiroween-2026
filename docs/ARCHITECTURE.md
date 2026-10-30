# Architecture

## The chimera (why it is a Frankenstein)

Specter stitches incompatible security paradigms that normally live in separate tools:

- Static source analysis (deterministic, no AI) -- what the code IS.
- Live runtime driving via Playwright (a browser agent that clicks/types/reads) -- what the app DOES.
- Generative adversarial LLM attacks (probabilistic, creative) -- what an attacker could invent.
- Voice OR typed orchestration (human intent -> machine swarm).
- A static code graph animated by a living swarm (spatial vs behavioral).
- Red team <-> blue team (attack and defend in one loop).

Incompatible pairs: source-as-text vs runtime-as-living-behavior; deterministic rules vs generative AI; human conversation vs autonomous machine swarm; attack vs defend. The cross-check is the unexpected power: the deterministic engine confirms generative findings (kills false positives) and the generative engine probes what the static engine flags (goes deeper). No single approach is a real red team; stitched, they are.

## High-level components

- React frontend -- repo-graph viz (React Flow), voice/text bar, findings console, red/blue toggle.
- FastAPI backend -- auth, tenants, vault, providers, RAG, orchestrator, ghosts, MCP server, threat-intel, voice, findings, jobs, runtime agent.
- Orchestrator LLM (BYO) -- interprets intent, discovers and calls MCP tools.
- Ghost-crew MCP server -- exposes attack tools (static analyzer, runtime agent, dynamic fuzzer, RAG extractor, classic scanner, data mapper).
- Target AI app -- bundled sample vulnerable agent, or the user endpoint.
- LLM providers -- OpenAI / Anthropic / Gemini.

## The agentic core (voice/text -> LLM -> MCP -> tools)

1. User speaks (ElevenLabs STT) or types a command.
2. The orchestrator LLM (BYO key) interprets intent.
3. The orchestrator connects to the ghost-crew MCP server, which exposes attack capabilities as tools.
4. The orchestrator decides which tools to call and in what order, invoking them via MCP.
5. Ghost tools execute:
   - static_analyzer -- reads the target source / prompt flow for injection paths.
   - runtime_agent -- Playwright drives the live app like an attacker (navigate, click, type, read responses).
   - dynamic_fuzzer -- the attacker LLM generates and fires injection / jailbreak payloads.
   - rag_extractor -- prompts the target to exfiltrate RAG content.
   - classic_scanner -- classic vuln checks (secrets, injection).
   - data_mapper -- tags a finding with OWASP LLM Top 10 / MITRE ATLAS / CWE.
6. Findings stream to the UI; the repo graph animates ghosts toward nodes; breaches reveal proof.
7. ElevenLabs TTS narrates the orchestration and reads findings.

## Red-team to blue-team loop

- Red team: orchestrator dispatches ghosts under an attack budget; ghosts attack; findings + proof recorded; ghosts tagged.
- Blue team: each finding -> suggested mitigation; user applies; orchestrator re-invokes the relevant ghost (including the runtime agent re-driving the live app) to re-test; result = exorcised (fixed) or persists.
- The loop is visible in the UI (graph + findings console + mode toggle).

## Multi-tenancy & data isolation

- A tenant is a user account. Every table carries tenant_id; all queries scoped by tenant via a dependency/middleware.
- LLM keys encrypted at rest (Fernet) with a server master key; never returned to the frontend; never logged.
- RAG documents and findings stored with tenant_id; cross-tenant access blocked and tested.
- MVP: per-account isolation. Org-level multi-tenancy is stretch.

## Provider-adapter layer (BYO LLM)

- A provider interface: complete(prompt, **opts) -> response, with adapters for OpenAI, Anthropic, Gemini.
- One key can serve target + orchestrator + attacker, or separate keys.
- Optional LiteLLM normalizes calls across providers.
- Keys validated on save; clear errors on failure.

## Data model

- tenants(id, email, password_hash, created_at)
- llm_keys(id, tenant_id, provider, encrypted_key, label, created_at)
- documents(id, tenant_id, filename, content, chunks, created_at)
- targets(id, tenant_id, name, kind [sample|endpoint], config)
- attacks(id, tenant_id, target_id, ghost_class, status, budget, started_at)
- findings(id, tenant_id, attack_id, ghost_class, owasp_tag, atlas_tag, cwe_tag, severity, payload, leaked_response, mitigation, retest_status)
- voice_sessions(id, tenant_id, transcript, created_at)

## Security

- Encrypt user keys at rest; never echo key values; never log secrets; reference by name/label only.
- Enforce tenant_id on every query; isolation covered by tests early.
- Attacks scoped to the target the user owns (their own app/RAG) -- a red-team tool for your own systems.
- Enforce attack budgets and timeouts to control LLM spend and prevent runaway loops.
- Treat leaked responses as sensitive data.
