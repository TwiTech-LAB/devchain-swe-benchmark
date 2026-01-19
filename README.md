# Devchain SWE-bench Results

Benchmark results comparing multi-agent vs single-agent LLM setups on [SWE-bench Verified](https://www.swebench.com/).

![SWE-bench Verified Results](results.png)

## Results Summary

|                  | Single-Agent | 2-Agent  | Difference |
| ---------------- | :----------: | :------: | ---------: |
| **Resolved**     |     80%      |   90%    |       +10pp |
| **Avg Time**     |   3.5 min    | 7.8 min  |       2.2x |
| **Unique Wins**  |      0       |    10    |        +10 |

- **Single-Agent:** Claude Opus 4.5 (Brainstormer)
- **2-Agent:** Claude Opus 4.5 (Brainstormer) + GPT-5.2 (Code Reviewer)

100 instances from SWE-bench Verified, evaluated with official harness.

## Key Findings

1. Code Review adds +10 percentage points to resolution rate
2. 2.2x time investment yields 12.5% more resolutions
3. 2-Agent setup never performed worse (zero regressions)
4. Largest impact on sphinx-doc (0% → 86%) and pylint (0% → 100%)

## Repository Structure

```
├── claude-100/                    # Single-Agent results
│   ├── test.json                  # Test configuration
│   ├── instances.txt              # 100 instance IDs tested
│   ├── devchain.claude-100.json   # SWE-bench evaluation results
│   └── results/
│       └── <instance_id>/
│           ├── result.json        # Agent comments, timing, status
│           └── patch.diff         # Generated patch
│
└── claude-codex-100/              # 2-Agent results
    └── (same structure)
```

## Result Files

Each `result.json` contains:

```json
{
  "instance_id": "django__django-11095",
  "status": "completed",
  "patch": "diff --git a/...",
  "duration_seconds": 160.9,
  "epic_comments": [
    {
      "author_name": "Brainstormer",
      "content": "## Fix Summary\n..."
    },
    {
      "author_name": "Code Reviewer",
      "content": "Code review PASSED..."
    }
  ]
}
```

## Resolution by Repository

| Repository              | Single | 2-Agent | Improvement |
| ----------------------- | :----: | :-----: | ----------: |
| pylint-dev/pylint       |   0%   |  100%   |       +100% |
| sphinx-doc/sphinx       |   0%   |   86%   |        +86% |
| sympy/sympy             |  81%   |   88%   |         +7% |
| django/django           |  91%   |   94%   |         +3% |
| pytest-dev/pytest       | 100%   |  100%   |          0% |
| scikit-learn/scikit-learn | 67%  |   67%   |          0% |
| pallets/flask           | 100%   |  100%   |          0% |
| astropy/astropy         |  33%   |   33%   |          0% |

## Methodology

Following official SWE-bench protocol. Agents receive:

| Information             | Provided |
| ----------------------- | :------: |
| Problem statement       |   Yes    |
| Hints text              |   Yes    |
| FAIL_TO_PASS test names |    No    |
| PASS_TO_PASS test names |    No    |
| Test patch              |    No    |

Evaluation performed with official SWE-bench harness.

## Tools

Results generated using [Devchain](https://github.com/twitech/devchain) - open source platform for orchestrating AI coding agents.

## License

MIT
