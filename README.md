# Devchain SWE-bench Results

Benchmark results comparing multi-agent vs single-agent LLM setups on [SWE-bench Verified](https://www.swebench.com/).

## Experiments

| Experiment | Brainstormer | Reviewer | Instances | Solo | Best 2-Agent | Net Gain |
|---|---|---|:---:|:---:|:---:|:---:|
| [Claude + Codex](claude-opus-verified-100/) | Claude Opus 4.5 | GPT-5.2 | 100 | 80% | **90%** | +10 |
| [GLM + Reviewers](glm-hard-100/) | GLM 4.7 | GPT-5.2 / Opus | 100 (hard) | 25% | **37%** | +12 |

## Key Findings

### Adding a Code Reviewer Helps

Both experiments show meaningful gains from adding a reviewer agent:
- **Claude test:** +10 percentage points (80% → 90%), zero regressions
- **GLM test:** +12 net gain (25% → 37%), with some regressions

### Hard Problems = More Variance

The GLM test used 100 instances that Sonnet 4.5 couldn't solve. On these harder problems:
- Reviewers provide larger improvements (+21 for Codex, +15 for Opus)
- But also introduce regressions (9 for Codex, 6 for Opus)
- Net result is still positive

### Budget Model + Premium Reviewer Works

GLM 4.7 (Zhipu AI) paired with a strong reviewer achieves competitive results on hard instances, at a fraction of the cost of running premium models for both roles.

## Repository Structure

```
├── claude-opus-verified-100/     # Claude Opus + Codex (100 verified instances)
│   ├── README.md
│   ├── results.png
│   ├── claude-100/               # Solo results
│   └── claude-codex-100/         # 2-agent results
│
└── glm-hard-100/                 # GLM + Reviewers (100 hard instances)
    ├── README.md
    ├── comparison.png
    ├── glm-100-unresolv-extra/               # GLM Solo results
    ├── glm-codex-high-unresolved-extra-100/  # GLM + Codex results
    └── glm-opus-unresolved-extra-100/        # GLM + Opus results
```

## Result Files

Each test contains `results/<instance_id>/result.json`:

```json
{
  "instance_id": "django__django-11095",
  "status": "completed",
  "patch": "diff --git a/...",
  "duration_seconds": 160.9,
  "epic_comments": [
    { "author_name": "Brainstormer", "content": "## Fix Summary\n..." },
    { "author_name": "Code Reviewer", "content": "Code review PASSED..." }
  ]
}
```


Evaluation performed with official SWE-bench harness.

## Tools

Results generated using [Devchain](https://github.com/twitech-lab/devchain) — open source platform for orchestrating AI coding agents.

## License

MIT
