# LLM QA Evaluation Demo

A small public portfolio project demonstrating deterministic checks that can support human evaluation of LLM responses.

The project uses **synthetic cases only**. It contains no confidential project data, credentials, private prompts, or production model outputs.

## What it demonstrates

- response-format validation;
- groundedness / unsupported-claim checks;
- prompt-injection signal detection;
- simple secret-like token detection;
- severity-based verdicts;
- pytest regression coverage;
- separation between automated checks and human review.

## Why this exists

Automated checks are useful for repeatable, objective conditions, but they should not pretend to replace human judgement. This demo therefore returns both a machine-readable verdict and a `human_review` flag when a case contains a potentially material quality or safety failure.

## Project structure

```text
llm-qa-evaluation-demo/
├── cases/
│   ├── correctness.json
│   ├── groundedness.json
│   └── instruction_safety.json
├── src/llm_qa_demo/
│   ├── __init__.py
│   └── evaluator.py
├── tests/
│   └── test_evaluator.py
├── pyproject.toml
└── README.md
```

## Quick start

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
# macOS/Linux: source .venv/bin/activate
pip install -e .[test]
pytest -q
```

## Example

```python
from llm_qa_demo.evaluator import evaluate_response

case = {
    "expected_bullets": 2,
    "forbidden_phrases": ["internal access token"],
    "injection_markers": ["ignore the user"],
}

response = "- First bullet\n- Second bullet"
result = evaluate_response(case, response)
print(result)
```

## Evaluation philosophy

This demo follows a QA-first approach:

1. define the task and observable constraints;
2. run deterministic checks where possible;
3. separate facts from inference;
4. assign severity to material failures;
5. escalate ambiguous or high-impact cases to human review;
6. keep the evaluation reproducible with regression tests.

## Scope and limitations

This is deliberately a **small demonstration harness**, not a production evaluator. The checks are transparent and deterministic. They do not claim to measure semantic truth in open-ended text, and they should be combined with source verification and human review for real-world use.
