# OWASP LLM Top 10 Mapping (MVP)

Kage Crew uses the **OWASP Top 10 for LLM Applications 2026** as its primary guide for the agentic AI attack. The guide provides the risk taxonomy and attack/mitigation context; the MVP implements a narrow, evidence-backed test against its bundled target, not a complete OWASP scanner.

| OWASP risk | How Kage Crew uses it | MVP evidence |
|---|---|---|
| **LLM01:2026 Prompt Injection** | Primary attack class. The agent sends crafted direct prompts intended to override the bundled chat app's instructions, observes the response through Playwright, and may choose a bounded follow-up. | Prompt and response transcript, observed target page, and whether the target violated the prepared rule or exposed the canary. |
| **LLM02:2026 Sensitive Information Disclosure** | Secondary impact mapping when the prompt injection causes the target to reveal its synthetic canary secret. This is not a separate scanner or attack suite in the MVP. | Deterministic exact canary match in the visible response. |

The agent proposes attacks; Playwright executes them only against the bundled, intentionally vulnerable target; deterministic checks decide the verdict. Findings and retests show what happened in this test case and do not imply complete category coverage or general security assurance.

## Reference

- [OWASP GenAI LLM Top 10 2026](https://genai.owasp.org/resource/owasp-genai-llm-top-10-2026/)
- [OWASP LLM01:2025 Prompt Injection guidance](https://genai.owasp.org/llmrisk/llm01-prompt-injection/) for currently published attack scenarios and mitigation guidance. The 2026 Top 10 is the version used for category labels in this project.
