# Milestones (MVP -> Final)

## M0 -- Foundation (days 1-4)
- Monorepo: backend (FastAPI) + frontend (React) skeleton.
- Auth + tenant model; encrypted key vault; provider adapter (one provider).
- A bundled sample vulnerable target agent (a simple RAG chatbot with known injection weaknesses).
- Basic UI shell.

## M1 -- MVP / the winning demo (days 5-12)
- Repo-graph visualization with ghosts attacking (React Flow + Framer Motion).
- Orchestrator LLM + MCP ghost tools: static analyzer, dynamic fuzzer, RAG extractor, data mapper.
- 3 AI attack classes (prompt injection, jailbreak, RAG exfiltration) + 1-2 classic (secrets, basic injection).
- OWASP / ATLAS / CWE local mapping; findings + proof.
- Blue-team mitigation + re-test (ghost re-attacks).
- ElevenLabs STT commands + TTS narration, plus typed commands (speak or type).
- The full red -> blue loop end to end.
This is what gets demoed and judged.

## M2 -- Playwright runtime limb + breadth (days 13-20)
- Add the Playwright runtime agent: drives the live target app like an attacker (navigate, click, type, read DOM/responses). This completes the chimera (source vs runtime vs generative).
- Multi-provider (Anthropic, Gemini).
- More attack classes (agent/tool hijack, insecure output, RAG poisoning).
- Haunted UI polish; voice orchestration polish.
- PBT suite; hooks + steering + dev-time MCP finalized.

## M3 -- Final product (days 21-30)
- Live NVD/OSV threat-intel.
- Classic vuln suite; persistent ghost-crew memory (stretch); org tenancy (stretch).
- Deployment (Vercel + Render/Koyeb) with the Playwright runtime as a headless browser service.
- Observability; demo video; writeup; bonus blog/social.

## The final product

A deployed, multi-tenant, voice-orchestrated AI red-team/blue-team workbench. A developer connects their own LLM and RAG, speaks or types to unleash a ghost crew that attacks their app via an LLM + MCP agentic loop -- reading the code, driving the live app with Playwright, and generating creative exploits -- watches ghosts breach their repo graph, and exorcises findings with proven re-tests. Every result is mapped to real OWASP LLM / ATLAS / CWE classifications.
