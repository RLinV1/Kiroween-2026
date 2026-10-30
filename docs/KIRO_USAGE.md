# Kiro Usage

Specter is built with Kiro and demonstrates deep, honest usage across the feature set. This is both a scoring lever and the required writeup.

## Spec-driven development (UI)
Structured requirements, design, and tasks for the frontend. Kiro implements the repo-graph UI, findings console, and voice/text bar from specs. See `.kiro/specs/specter/`.

## Vibe coding (red-team / agent logic)
The orchestrator and ghost logic are built conversationally and iteratively, because "attack like a red team" is abstract and benefits from injected human judgment. This is the inverted pattern used by the 2025 Frankenstein winner: specs for the UI, vibe coding for the hard reasoning logic. We document the reasoning.

## Agent hooks
A hook auto-generates a new ghost / MCP tool skeleton plus its detection rule whenever a new attack class is added to a spec. This is the proven 2025 winning pattern (the InteractionKit hook that auto-generated gesture logic), applied to attack tools.

## MCP (dev-time + runtime)
- Dev-time: Kiro connects to security reference data (OWASP, MITRE, NVD docs) during the build.
- Runtime: the ghost crew itself is an MCP server -- the orchestrator LLM discovers and invokes attack tools via MCP at runtime. MCP is load-bearing in the product, not just the build, which doubles the story.

## Steering docs
`.kiro/steering/` enforces architecture, attack-rule consistency, and code style across the build.

## Property-based testing (PBT)
PBT verifies that detection rules do not false-positive and that mitigations hold against property-based injection payloads -- a genuine, impressive use that a wrapper project cannot replicate.

## Multi-agent orchestration
The ghost crew (static analyzer, runtime agent, dynamic fuzzer, RAG attacker, classic scanner, data mapper) plus the orchestrator form an agent squad, orchestrated through MCP.

## Honest failures (documented)
We document where Kiro's eagerness to pass tests caused issues (over-broad try/except, import-only tests, None-returns to satisfy types) and how we audited for them -- echoing the honest engineering mastery the 2025 winner showed.
