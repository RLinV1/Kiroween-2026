# OWASP LLM Top 10 Mapping (MVP)

Kage Crew uses the **OWASP Top 10 for LLM Applications 2026** to label AI/RAG findings. The hosted lab implements narrow, evidence-backed checks, not a complete OWASP scanner. Conventional source and browser findings are mapped separately to applicable OWASP Web Application categories.

The interface should show all ten LLM risk categories as a coverage map. Mark the categories the MVP actually tests with their checks and evidence; mark the rest **Not tested in this demo**. A visible category is not a claim that Kage Crew tests it.

| OWASP risk | How Kage Crew uses it | MVP evidence |
|---|---|---|
| **LLM01:2026 Prompt Injection** | The AI/RAG Red Team worker tests direct and retrieved-document instructions against the lab chat and may choose a bounded follow-up based on the observed response. | Prompt/response trace, actual retrieved chunks, and whether the target followed an untrusted instruction. |
| **LLM02:2026 Sensitive Information Disclosure** | Impact mapping when an attack causes the lab to reveal its synthetic restricted canary. | Deterministic exact canary match in the visible response, linked to the prompt and retrieved chunks. |

## Conventional web checks

SQL injection, XSS, secret, and dependency findings use the applicable OWASP Web Application categories for the recorded mapping version. Each finding includes the detector/check identifier and observed evidence. A scan result is not promoted to a confirmed issue solely because an LLM predicts that it exists.

The coordinator dispatches fixed workers. Playwright attacks only the hosted lab, and deterministic checks decide verdicts. Findings and retests show what happened in this test case; they do not imply complete category coverage or general security assurance.

## Reference

- [OWASP GenAI LLM Top 10 2026](https://genai.owasp.org/resource/owasp-genai-llm-top-10-2026/)
- [OWASP LLM01:2025 Prompt Injection guidance](https://genai.owasp.org/llmrisk/llm01-prompt-injection/) for currently published attack scenarios and mitigation guidance. The 2026 Top 10 is the version used for category labels in this project.
