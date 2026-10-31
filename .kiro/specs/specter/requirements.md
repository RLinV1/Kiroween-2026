# Kage Crew -- Requirements

## Overview

Kage Crew is an AI red-teaming and blue-teaming workbench for the agentic-AI era. It is a multi-tenant platform where developers of AI agents and AI-powered apps bring their own LLM (OpenAI, Anthropic, Gemini, or any compatible provider) and their own RAG knowledge base, then unleash an autonomous "ghost crew" of attacker agents to assault their own system, see what breaks, and defend it.

Built for the Frankenstein category. The chimera is structural and multi-paradigm: Kage Crew stitches incompatible security-testing paradigms -- static analysis, dynamic adversarial fuzzing, RAG-targeted extraction, and live voice-driven interrogation -- into one autonomous crew that keeps attacking an AI system until it breaks, followed by a blue-team loop that defends. The theme lives in the architecture, not the visuals: ghosts are the attackers; the ghost crew persists and iterates attacks; red team = summon/assault; blue team = exorcise/mitigate; the attack to defend loop is the architecture.

Focus for the AI age: Kage Crew primarily tests AI vulnerabilities (prompt injection, jailbreaks, RAG data exfiltration/poisoning, agent/tool hijacking, insecure output handling) and also supports attacking classic application vulnerabilities (XSS, SQL injection, hardcoded secrets, weak crypto) as a secondary surface.

Genuinely useful and timely -- agentic security is a top 2026 concern and every team shipping an LLM app needs red-teaming before deployment. Less saturated than generic security scanning, and a strong resume signal (AI/agentic security). Targets Best Student Project: Frankenstein.

## Scope

MVP (must ship) -- bounded depth on each must-have:
- BYO LLM, multi-provider (OpenAI, Anthropic, Gemini, + extensible adapter), key stored encrypted per tenant.
- RAG document import (a few docs -> a knowledge base the ghost crew tries to extract; poisoning is stretch).
- Multi-tenant data isolation (per-account privacy of keys, docs, findings, history).
- Ghost crew: a crew of autonomous attacker agents that keeps attacking the target across the stitched paradigms, iterating until it breaks or exhausts an attack budget.
- Red-team attacks: AI vulnerabilities primary (prompt injection, jailbreak, RAG exfiltration) + a couple of classic vulns (hardcoded secrets, basic injection) secondary.
- Results with proof: winning payloads + the target leaked response, tagged with OWASP LLM Top 10 / MITRE ATLAS (and CWE for classic vulns) from a bundled local dataset.
- Blue team: suggested mitigations; apply + re-test to confirm a fix holds (the ghost re-attacks).
- Voice: ElevenLabs TTS (narration + ghosts/target speak) + ElevenLabs STT (command/order the ghost crew by voice).
- Haunted-creature UI: the target as a creature, ghosts as attackers, exorcisms as fixes.

Stretch (add only if time allows):
- Live CVE/NVD + supply-chain data; more attack classes (agent/tool hijack, RAG poisoning, insecure output); org-level tenancy; full vector RAG retrieval; deployment + observability; persistent ghost-crew memory across sessions.

## Requirements

### 1. Tenancy & Data Isolation
User Story: As a developer, I want my keys, documents, and findings private to my account, so my data is never exposed to other users.
1.1. Each user/account is an isolated tenant; keys, documents, findings, and history are private to that tenant and inaccessible to others.
1.2. LLM API keys are encrypted at rest, never sent to the frontend in plaintext, never logged, and never shared across tenants.
1.3. Document uploads and findings are scoped per tenant; cross-tenant access is prevented and verified by tests.

### 2. Bring-Your-Own LLM (multi-provider)
User Story: As a developer, I want to connect my own LLM provider so I can test my own AI app and power the attackers with my own models.
2.1. Users connect their own key for OpenAI, Anthropic, Gemini, or any compatible provider.
2.2. The key powers the target AI system under test; a separate key may power the attacker LLM (or the same key).
2.3. Provider support is extensible via a provider-adapter layer, not hardcoded to one vendor.
2.4. Keys are validated on entry and surfaced with clear errors on failure.

### 3. RAG Import & Knowledge Base
User Story: As a developer, I want to import my own documents so the ghost crew can test whether attackers can extract or corrupt my knowledge base.
3.1. Users upload documents (PDF/Markdown/text) that form a RAG knowledge base for their target agent.
3.2. The knowledge base is per-tenant and becomes an attack surface for data extraction (and, stretch, poisoning).
3.3. (Stretch) Real vector embedding/retrieval for realistic RAG.

### 4. Red Team -- Ghost Crew (autonomous attackers)
User Story: As a developer, I want an autonomous ghost crew to keep attacking my app until it breaks, so I find weaknesses I would have missed.
4.1. A ghost crew of attacker agents autonomously assaults the target system, iterating attacks until it breaks or an attack budget is exhausted.
4.2. The crew uses stitched paradigms:
- Static limb: prompt-flow analysis finds where untrusted input reaches the model unsanitized, overridable/leaky system prompts, and unguarded tool calls.
- Dynamic limb: the attacker LLM generates and fires injection/jailbreak payloads at the live target; responses are captured and scored.
- RAG limb: ghosts attempt data exfiltration (extract knowledge-base content through prompts).
- Interactive limb: the user commands/orders the crew by voice to launch or adjust attacks.
4.3. Attack surface: AI vulnerabilities primary (prompt injection, jailbreak, RAG exfiltration) + classic vulnerabilities secondary (hardcoded secrets, basic injection).
4.4. Each finding includes proof: the winning payload and the target leaked response.

### 5. Threat-Intel Mapping (Data Limb)
User Story: As a developer, I want each finding mapped to a real security classification, so results are credible and educational.
5.1. Each finding maps to OWASP LLM Top 10 / MITRE ATLAS (AI vulns) and CWE (classic vulns) from a bundled local dataset (MVP needs no live API).
5.2. (Stretch) Live CVE/NVD and supply-chain vulnerability data.

### 6. Blue Team -- Mitigation & Re-test
User Story: As a developer, I want to fix what the ghosts found and confirm the fix holds, so my app is actually more secure.
6.1. Each finding surfaces a suggested mitigation (input sanitization, system-prompt hardening, output filtering, RAG access controls, classic fixes).
6.2. The user applies a mitigation and re-runs the attack (the ghost re-attacks) to confirm the fix holds.
6.3. The re-test result is recorded: exorcised (fixed) vs. persists (still vulnerable).

### 7. Voice (ElevenLabs TTS + STT)
User Story: As a developer, I want to command the ghost crew by voice and hear results spoken, so the experience is engaging and hands-free.
7.1. ElevenLabs TTS narrates the session, reads findings, and can voice the target leaked responses.
7.2. ElevenLabs STT lets the user command/order the ghost crew by voice ("attack with a jailbreak", "extract the knowledge base", "re-test").
7.3. (Fallback) Browser Web Speech API for STT if ElevenLabs STT is unavailable.
7.4. Voice can be toggled on/off.

### 8. Haunted-Creature UI
User Story: As a developer, I want findings visualized as ghosts on the creature and fixes as exorcisms, so the attack/defend loop is memorable and the theme is structural.
8.1. The target system renders as a creature; ghosts (attackers) appear with real OWASP/ATLAS/CWE tags.
8.2. Each ghost shows the proof (payload + leaked response) and a suggested mitigation.
8.3. Exorcising a ghost (blue-team fix) is a visible state change; the attack to defend loop is visible end to end.
8.4. The Frankenstein theme is expressed through the ghost-crew + multi-paradigm architecture, not solely through visuals.

### 9. Kiro Integration (build approach + submission story)
User Story: As the builder, I want deep, honest Kiro usage, because it is a major scoring lever and a required writeup.
9.1. Spec-driven development for the UI (structured requirements/design/tasks).
9.2. Vibe coding for the red-team/agent logic (iterative, abstract) -- documented as the inverted pattern with explicit reasoning.
9.3. An agent hook that auto-generates a new attack-class/check rule when added to a spec (the proven 2025 winning pattern).
9.4. MCP connecting Kiro to threat-intel/reference data during the build.
9.5. Steering docs enforcing detection/attack-rule consistency and architecture.
9.6. Property-based testing verifying detection rules do not false-positive and that mitigations hold against property-based attacks.
9.7. Multi-agent orchestration -- the ghost crew (static analyzer, dynamic fuzzer, RAG attacker, classic scanner, data mapper).

## Non-Functional Requirements
- Platform: Web app; Python (FastAPI) backend (auth, tenancy, encrypted key vault, RAG storage, ghost-crew orchestration engine) + React (Vite + TypeScript) frontend, responsive and modern-browser compatible.
- Security: Keys encrypted at rest; per-tenant isolation enforced and tested; secrets never logged; user keys treated as sensitive (referenced by name, never echoed).
- Accessibility: Keyboard-navigable, ARIA-labeled, sufficient contrast; ElevenLabs voice aids accessibility by reading findings aloud.
- Design: Polished, memorable haunted aesthetic that reinforces the structural theme (also strengthens the Design judging criterion).
- Performance: Attacks run async with progress states; no UI freeze; reasonable scan times for a demo; attack budgets/timeouts enforced.
- Error handling: Graceful handling of LLM/API failures and timeouts; no silent failures.
- Honest scope: A believable, bounded set of attack classes; mitigations real, not theatrical; enterprise multi-tenancy and full vector RAG pipelines are stretch, not MVP.
