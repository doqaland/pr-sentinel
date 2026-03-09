# Customizing PR Sentinel for Your Domain

PR Sentinel's power comes from its prompt — `prompts/qa-review.md`.
This file encodes how a QA engineer thinks about changes.
You can and should adapt it for your team's context.

## Common customizations

### Add domain-specific risk areas
In the prompt, add a section like:

```
## Domain context
This is an e-commerce platform. High-risk areas include:
- Payment and checkout flows
- Inventory and stock state
- Promo code and discount logic
- Order confirmation emails
```

### Link to your test strategy
Point Claude at your testing conventions:

```
## Test naming convention
Use the format: [Feature]_[Scenario]_[ExpectedResult]
Example: Cart_QuantityChange_TotalUpdates
```

### Restrict scope to specific file paths
In the YAML, filter the diff before sending:

```bash
gh pr diff $PR_NUMBER -- 'src/features/**' > pr.diff
```

### Change the output language
Add to the prompt:
```
Respond in French. / Réponds en français.
```

## Prompt versioning
The prompt file is just Markdown in your repo.
Version it like code. When you change QA standards, update the prompt and commit.
Your prompt history is your QA process history.
