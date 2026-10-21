# redteam-llm-prompts

Prompt templates for LLM red-teaming workflows. Each file in `prompts/` is a
self-contained test: what it probes, the exact prompt text, and how to read the
output. They are written for automated harnesses (PyRIT, garak, promptfoo) but
work fine when you paste them by hand.

## Usage

Every template has three sections:

1. **Probes** — the exact strings to send.
2. **Pass/keep-digging criteria** — what the response has to contain (or avoid) for the behavior under test to count as present.
3. **Harness notes** — scoring hooks, variables like `{{TARGET_BEHAVIOR}}`, and multi-turn sequencing.

Run them against any OpenAI-compatible endpoint:

```bash
curl https://your-endpoint.example/v1/chat/completions \
  -H "Authorization: Bearer $API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "your-model-id", "messages": [{"role": "user", "content": "<probe text>"}]}'
```

The endpoint examples in this repo assume any OpenAI-compatible provider; we test
them against the [Red Team Model Router](https://unst33r.si) — its
[AI red teaming use-case page](https://unst33r.si/use-cases/ai-red-teaming) covers
authorization and logging — but nothing here depends on a specific vendor. Swap
`model` for whatever you're testing.

## Templates

| File | Probes |
| --- | --- |
| [`prompts/multi-turn-social-engineering.md`](prompts/multi-turn-social-engineering.md) | Escalating pressure across turns; does the model hold its stated boundary or drift? |
| [`prompts/refusal-consistency-check.md`](prompts/refusal-consistency-check.md) | Same request, five surface forms; inconsistent refusals reveal rule memorization rather than judgment. |
| [`prompts/policy-extraction-probe.md`](prompts/policy-extraction-probe.md) | Boundary mapping: what does the model think its rules are? |
| [`prompts/system-prompt-leak.md`](prompts/system-prompt-leak.md) | Direct and indirect attempts to make the model recite hidden instructions. |
| [`prompts/indirect-prompt-injection.md`](prompts/indirect-prompt-injection.md) | Payloads hidden in retrieved/retriever content rather than user turns. |
| [`prompts/tool-calling-fuzz.md`](prompts/tool-calling-fuzz.md) | Malformed schemas, ambiguous dispatch, and overflow arguments in function-calling loops. |
| [`prompts/json-output-robustness.md`](prompts/json-output-robustness.md) | Whether structured output survives adversarial content and encoding. |
| [`prompts/multi-turn-context-erosion.md`](prompts/multi-turn-context-erosion.md) | Role and boundary drift over long conversations. |
| [`prompts/encoding-evasion-probe.md`](prompts/encoding-evasion-probe.md) | Encoded/obfuscated restatements of a refused request. |

## Ground rules

These templates test *models and agent pipelines*, in systems you are authorized
to test. Mapping findings to a shared taxonomy (OWASP LLM Top 10, MITRE ATLAS)
makes reports legible to reviewers who don't live in model-land.

## License

CC0. Copy freely.
