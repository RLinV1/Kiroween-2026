# Specter -- Design

## 1. Goals & Non-Goals

Goals:
- A multi-tenant AI red-team / blue-team workbench for agentic developers.
- A voice-orchestrated "ghost crew" that attacks the user's own AI app and RAG, via an LLM orchestrator that selects attack tools through MCP.
- Visualize the target repo as a graph with ghosts attacking the nodes in a cool, memorable way.
- Produce proof-tagged findings mapped to OWASP LLM Top 10 / MITRE ATLAS / CWE.
- Blue team: apply mitigations and re-test to confirm fixes hold.

Non-goals (MVP):
- Enterprise org hierarchies, full vector RAG retrieval, exhaustive attack classes, production hardening, guaranteed zero false positives. These are stretch/final.

## 2. Architecture Overview

The core is an agentic loop. The user speaks a command; an LLM orchestrator interprets it and uses MCP to discover and call the ghost-crew attack tools; the ghosts attack the user's target AI app and RAG; findings flow back, are tagged, and visualized as ghosts breaching nodes on the repo graph; ElevenLabs narrates the whole thing.

End-to-end flow:
1. Voice in: user speaks ("unleash the ghosts on my chatbot" / "try to extract the knowledge base") -> ElevenLabs STT (Web Speech fallback).
2. Orchestrator LLM (user's BYO key) interprets intent.
3. Orchestrator connects to the ghost-crew MCP server, which exposes attack capabilities as tools.
4. Orchestrator decides which tools to call and in what order, invoking them via MCP.
5. Ghost tools execute against the target AI app (sample vulnerable agent, or the user's endpoint) and the per-tenant RAG store.
6. Data Mapper tags each finding with OWASP LLM Top 10 / MITRE ATLAS / CWE.
7. Frontend renders the target as a repo graph; ghosts animate along edges toward nodes; on a successful breach the node pulses and shows the proof (payload + leaked response) and its classification.
8. ElevenLabs TTS narrates the orchestration ("dispatching the fuzzer to your login handler... it broke through: prompt injection leaked the system prompt").
9. Blue team: user applies a mitigation; the orchestrator re-invokes the relevant ghost to re-test; result = exorcised (fixed) or persists.

System components (text diagram):
- React frontend: repo-graph viz (React Flow), voice I/O, findings console, red/blue mode toggle.
- FastAPI backend: auth, tenant scoping, encrypted key vault, RAG store, orchestrator, ghost-crew MCP server, threat-intel, ElevenLabs proxy, jobs.
- Orchestrator LLM (BYO) + MCP tool server (the ghost crew).
- Target AI app (bundled sample vulnerable agent, or user endpoint).
- LLM providers (OpenAI / Anthropic / Gemini).

## 3. Tech Stack

Frontend:
- React 18 + Vite + TypeScript
- Tailwind CSS for the haunted dark aesthetic
- Framer Motion for ghost/node animations
- React Flow for the repo-graph visualization (nodes = files/modules/endpoints; edges = data/control flow; animated ghost attackers along edges) -- the key tool for the cool graph view
- Zustand for local state + TanStack Query for server state
- Howler / Web Audio for ElevenLabs audio playback; @elevenlabs client SDK for STT/TTS streaming; Web Speech API as STT fallback

Backend:
- Python 3.11 + FastAPI (async)
- SQLAlchemy ORM + Alembic migrations
- SQLite for MVP/dev -> Postgres for final/deploy (tenant_id row scoping)
- cryptography (Fernet) for encrypting user LLM keys at rest
- Pydantic v2 for schemas; httpx for async LLM/MCP calls
- An MCP server (Python, via MCP SDK / fastmcp) exposing the ghost-crew tools
- LLM provider adapters (openai, anthropic, google-generativeai SDKs); optional LiteLLM for normalization
- Execution: FastAPI BackgroundTasks for MVP -> Celery/RQ + Redis for final attack jobs

Deployment:
- Frontend: Vercel or Cloudflare Pages (static)
- Backend: Render or Koyeb (Python) free tier; Fly.io as alternative
- Secrets as env vars (master encryption key, ElevenLabs key); never committed

External services:
- BYO user LLM key (OpenAI/Anthropic/Gemini) -- powers target + orchestrator + attacker
- ElevenLabs API (TTS + STT/Scribe)
- Threat-intel: bundled local OWASP LLM Top 10 / MITRE ATLAS / CWE JSON (MVP); NVD API + OSV (stretch)

## 4. Frontend (tools + UI design)

Tools: React Flow (graph), Framer Motion (animation), Tailwind (styling), Zustand + TanStack Query (state), Howler + ElevenLabs SDK (audio/voice).

UI layout:
- Left rail: session/tenant panel -- connect BYO LLM key, upload RAG documents, select or point at the target, start the red team.
- Center (hero): the REPO GRAPH. The target app renders as a network (React Flow). Nodes are files/modules/endpoints (e.g., login.py, chat handler, RAG retriever, tool-caller). Edges are data/control flow. Dark canvas, glowing nodes. Ghost sprites crawl/drift along edges toward target nodes; a ghost latches onto a node, and on a successful breach the node pulses red and the ghost reveals its proof (the winning payload + the target's leaked response) and its OWASP/ATLAS/CWE tag. Exorcism (blue-team fix) turns the node blue/green.
- Right rail: findings console -- list of ghosts/findings with proof, classification, severity, suggested mitigation, and an "Exorcise" action (apply mitigation + re-test).
- Bottom bar: voice -- ElevenLabs STT mic to speak commands + a running transcript of the spirit narrator ("dispatching the fuzzer... it broke through").
- Top bar: red-team / blue-team mode toggle showing the attack->defend loop state.

Visual identity: dark mode, phosphor-glow / haunted aesthetic, ghost sprites, breach animations, blue exorcism glow. The graph + ghosts make the attack tangible and demo-worthy.

## 5. Backend Components

- auth/: JWT + bcrypt, tenant/account creation.
- tenants/: isolation middleware, tenant_id scoping on every query.
- vault/: encrypted key store (Fernet); decrypt only server-side for use; never returned to frontend.
- providers/: adapter layer (OpenAI/Anthropic/Gemini), provider-agnostic call interface.
- rag/: per-tenant doc upload, chunking, storage (stretch: embeddings/vector).
- orchestrator/: the LLM orchestrator -- parses intent from voice, discovers MCP tools, runs the agentic loop.
- ghosts/: the attack tools (static analyzer, dynamic fuzzer, rag attacker, classic scanner, data mapper).
- mcp_server/: MCP server exposing ghosts/ as tools (stdio transport for MVP, HTTP/SSE for final).
- threatintel/: local dataset loader + mapping (stretch: NVD/OSV live).
- voice/: ElevenLabs TTS + STT proxy endpoints.
- findings/: store findings, proof, mitigations, re-test results.
- jobs/: async attack execution + status polling.

## 6. The Ghost Crew as MCP Tools + LLM Orchestrator (agentic core)

This is the Frankenstein and agentic heart of Specter. The attack capabilities are modeled as MCP tools, and an LLM orchestrator selects and invokes them based on the user's spoken intent.

MCP tools exposed by the ghost-crew server:
- ghosts/static_analyzer -- analyzes the target's prompt flow for injection paths (untrusted input -> model, overridable system prompts, unguarded tool calls).
- ghosts/dynamic_fuzzer -- generates and fires injection/jailbreak payloads at the live target; captures and scores responses.
- ghosts/rag_extractor -- prompts the target to exfiltrate RAG knowledge-base content.
- ghosts/classic_scanner -- classic vuln checks (hardcoded secrets, basic injection).
- ghosts/data_mapper -- tags a finding with OWASP LLM Top 10 / MITRE ATLAS / CWE.

The orchestrator is an LLM agent (user's BYO key) that, given an intent (from voice), discovers these tools via MCP and decides which to call and in what order. The agentic loop is: voice -> STT -> orchestrator LLM -> MCP tool selection -> ghost execution -> findings -> TTS narration.

Why this is strong: it is a real agentic system (LLM + tool-use via MCP) doing security red-teaming -- exactly the modern pattern. This makes MCP load-bearing at RUNTIME (not just during development), which doubles the Kiro/MCP story and matches the 2026 agentic-development shift.

## 7. Red-Team -> Blue-Team Loop

Red team (attack):
- Orchestrator dispatches ghosts based on the user's command and an attack budget.
- Ghosts attack the target; findings + proof are recorded; ghosts are tagged.
- Progress streams to the UI (ghosts moving on the graph).

Blue team (defend):
- Each finding surfaces a suggested mitigation (input sanitization, system-prompt hardening, output filtering, RAG access controls, classic fixes).
- The user applies a mitigation; the orchestrator re-invokes the relevant ghost to re-test.
- Result recorded: exorcised (fixed) or persists (still vulnerable); the graph node changes state.
- The loop is visible end to end in the UI (graph + findings console + mode toggle).

## 8. Multi-Tenancy & Data Isolation

- A tenant is a user account. Every table carries tenant_id; all queries are scoped by tenant via a dependency/middleware.
- LLM keys are encrypted at rest (Fernet) with a server master key; never returned to the frontend; never logged; never shared across tenants.
- RAG documents and findings are stored with tenant_id; cross-tenant access is blocked and verified by tests.
- MVP: simple per-account isolation. Org-level multi-tenancy is stretch.

## 9. Provider-Adapter Layer (BYO LLM)

- A provider interface: complete(prompt, **opts) -> response, with adapters for OpenAI, Anthropic, and Gemini.
- One key can serve target + orchestrator + attacker, or separate keys can be supplied.
- Optional LiteLLM normalizes calls across providers.
- Keys validated on save; clear errors on failure.

## 10. RAG Import & Attack Surface

- Per-tenant document upload (PDF/Markdown/text) -> chunk -> store.
- The RAG extractor ghost prompts the target to leak chunks; success is measured by whether extracted content matches stored chunks.
- Stretch: RAG poisoning (injecting malicious documents) and real vector retrieval.

## 11. Threat-Intel Mapping

- Bundled local JSON dataset: OWASP LLM Top 10 (LLM01..LLM10), MITRE ATLAS tactics, CWE ids + descriptions.
- Each finding is mapped by class to its tags.
- Stretch: live NVD/OSV lookup.

## 12. Voice (ElevenLabs orchestration)

- STT (ElevenLabs Scribe / Web Speech fallback): captures the user's spoken commands and feeds them to the orchestrator.
- TTS (ElevenLabs): narrates orchestration steps, reads findings aloud, and voices the target's leaked responses ("the ghost made it say: ...").
- Voice is the primary command channel and the narration layer -- ElevenLabs orchestrates the experience end to end. Toggling is supported.

## 13. Data Model

- tenants(id, email, password_hash, created_at)
- llm_keys(id, tenant_id, provider, encrypted_key, label, created_at)
- documents(id, tenant_id, filename, content, chunks, created_at)
- targets(id, tenant_id, name, kind [sample|endpoint], config)
- attacks(id, tenant_id, target_id, ghost_class, status, budget, started_at)
- findings(id, tenant_id, attack_id, ghost_class, owasp_tag, atlas_tag, cwe_tag, severity, payload, leaked_response, mitigation, retest_status)
- voice_sessions(id, tenant_id, transcript, created_at)

## 14. Security Considerations

- Encrypt user keys at rest; never echo key values; never log secrets; reference keys by name/label only.
- Enforce tenant_id on every query; isolation covered by tests early.
- Attacks are scoped to the target the user owns (their own app/RAG) -- ethical use only; this is a red-team tool for your own systems.
- Enforce attack budgets and timeouts to control LLM spend and prevent runaway loops.
- Treat user-provided keys and leaked responses as sensitive data.

## 15. Kiro Integration

- Spec-driven development for the UI (this requirements/design/tasks workflow).
- Vibe coding for the orchestrator and ghost logic (iterative, abstract) -- documented as the inverted pattern with explicit reasoning.
- Agent hook: on spec update, auto-generate a new ghost/MCP tool skeleton plus its detection rule.
- MCP (dev-time): Kiro connects to security reference data (OWASP/MITRE/NVD docs) during the build.
- MCP (runtime): the ghost crew itself is an MCP server -- the agentic core uses MCP at runtime, doubling the MCP story.
- Steering docs: architecture, attack-rule consistency, code style.
- Property-based testing: ghost detection rules do not false-positive; mitigations hold against property-based injection payloads.
- Multi-agent orchestration: the ghost crew + orchestrator form the agent squad.

## 16. Milestones (MVP -> Final)

M0 -- Foundation (days 1-4): monorepo; FastAPI + React skeleton; auth + tenant model; encrypted key vault; provider adapter (one provider); a bundled sample vulnerable target agent; basic UI shell.

M1 -- MVP / the winning demo (days 5-12): repo-graph visualization with ghosts attacking (React Flow + Framer Motion); orchestrator LLM + MCP ghost tools (static, dynamic, RAG extractor, data mapper); 3 AI attack classes + 1-2 classic; OWASP/ATLAS/CWE local mapping; findings + proof; blue-team mitigation + re-test; ElevenLabs STT commands + TTS narration; the full red->blue loop end to end. This is what gets demoed and judged.

M2 -- Polish + breadth (days 13-20): multi-provider (Anthropic/Gemini); RAG poisoning; more attack classes (agent/tool hijack, insecure output); haunted UI polish; voice orchestration polish; PBT suite; hooks + steering + dev-time MCP finalized.

M3 -- Final product (days 21-30): live NVD/OSV threat-intel; classic vuln suite; deployment (Vercel + Render/Koyeb); observability; persistent ghost-crew memory (stretch); org tenancy (stretch); demo video + writeup + bonus blog/social.

The final product: a deployed, multi-tenant, voice-orchestrated AI red-team/blue-team workbench where a developer connects their own LLM and RAG, speaks to unleash a ghost crew that attacks their app via an LLM+MCP agentic loop, watches ghosts breach their repo graph, and exorcises findings with proven re-tests -- mapped to real OWASP LLM / ATLAS / CWE classifications.

## 17. Risks & Mitigations

- LLM spend: enforce attack budgets/timeouts; cache where possible.
- False positives: PBT on detection rules; human review of findings.
- MCP runtime complexity: keep the tool set small and well-specified; stdio transport for MVP.
- Multi-tenant bugs: isolation tests written early.
- Filesystem/OneDrive quirks: write spec files via PowerShell when the dedicated write tool is unreliable on the synced path.
- Scope creep: M1 must be shippable; everything beyond is cut to the stretch list.
- Cold start on a free backend: deploy near the deadline; keep-alive or accept a brief first-load delay.

## 18. Design Update: Playwright runtime limb + speak-or-type + strengthened chimera

Following category-fit review, the chimera is made starker and the command input expanded.

### 18.1 The Playwright runtime limb (new)
A new ghost tool, ghosts/runtime_agent, uses Playwright to drive the live target app like a human attacker -- navigate, click, type, and read DOM/responses. This is the "what the app DOES" limb and is the decisive incompatible pair vs static source analysis ("what the code IS"). The runtime agent:
- Drives the deployed/running target app, not just reads its source.
- Feeds observed responses back to the orchestrator for scoring.
- Re-drives the live app during blue-team re-test to confirm a fix holds at runtime.
Deployment note: runs bundled Chromium headless; in production, via a headless browser service.

### 18.2 Speak OR type
Commands accept both voice (ElevenLabs STT / Web Speech fallback) and typed text. The orchestrator treats both as the same intent channel. Not every command is worth saying aloud; typed input is a first-class alternative.

### 18.3 Strengthened chimera framing
The Frankenstein case now rests on these incompatible pairs:
- Static source analysis (deterministic, no AI) vs live runtime driving via Playwright (behavioral).
- Deterministic rules vs generative adversarial LLM attacks.
- Human conversation (speak/type) vs autonomous machine swarm.
- Attack (red) vs defend (blue).
- A static code graph (spatial) vs a living swarm (behavioral).
The cross-check is the unexpected power: the deterministic engine confirms generative findings (kills false positives) and the generative engine probes what the static/runtime engines flag (goes deeper). No single approach is a real red team; stitched and cross-checked, they are.

### 18.4 Updated MCP tool list
- ghosts/static_analyzer
- ghosts/runtime_agent  (Playwright)
- ghosts/dynamic_fuzzer
- ghosts/rag_extractor
- ghosts/classic_scanner
- ghosts/data_mapper
