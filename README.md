# 🔍 PR Sentinel

**The QA engineer's eye on every pull request — powered by Claude AI.**

· Open source · MIT License

---

## The problem

Every team now has an AI that reviews code quality.  
Nobody has an AI that thinks like a QA engineer.

A code reviewer asks: *"Is this well written?"*  
A QA engineer asks: *"Does this do what the ticket said? What could break in production?"*

PR Sentinel adds the QA perspective to every PR — automatically, before QA is ever pinged.

---

## What it does

When a PR is opened or updated, PR Sentinel:

1. Reads the diff and the PR description / acceptance criteria
2. Sends it to Claude with a QA-focused prompt
3. Posts a structured QA analysis as a PR comment

The comment includes:
- **AC Coverage** — does the code match the acceptance criteria?
- **Risk level** — LOW / MEDIUM / HIGH
- **Functional gaps** — what could break that the developer didn't test
- **Suggested test scenarios** — concrete Given/When/Then cases
- **Regression risk** — which existing flows are touched
- **Open questions** — what QA needs answered before sign-off

---

## Setup — 3 steps

### 1. Add your Anthropic API key to GitHub Secrets

In your repo: `Settings → Secrets and variables → Actions → New secret`

Name: `ANTHROPIC_API_KEY`  
Value: your key from [console.anthropic.com](https://console.anthropic.com)

### 2. Copy the workflow and prompt into your repo

```bash
# From your repo root
mkdir -p .github/workflows prompts

curl -o .github/workflows/pr-sentinel.yml \
  https://raw.githubusercontent.com/Anasss/pr-sentinel/main/.github/workflows/pr-sentinel.yml

curl -o prompts/qa-review.md \
  https://raw.githubusercontent.com/Anasss/pr-sentinel/main/prompts/qa-review.md
```

### 3. Open a PR

PR Sentinel runs automatically. Look for the comment within ~60 seconds.

---

## Customization

The prompt file `prompts/qa-review.md` is the core of PR Sentinel.  
Edit it to add domain context, risk areas, or your team's test conventions.

See [docs/customization.md](docs/customization.md) for examples.

---

## Example output

See [examples/sample-output-feature.md](examples/sample-output-feature.md)  
and [examples/sample-output-bugfix.md](examples/sample-output-bugfix.md)

---

## Why Claude?

Claude's extended reasoning handles long diffs better than most models,
and its instruction-following makes the structured output format reliable.
You bring your own API key — no data leaves your control.

---

## License

MIT — use it, fork it, adapt it for your team.

---

*Built by [DoQALand](https://doqaland.com) — QA automation for teams who ship fast.*
