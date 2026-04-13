# skill: build-evidence-base

## Purpose
Given a spec or PRD page, traverse its `sources` and `dependents` connections and build (or update) the Evidence Base table — the structured record of what informed this spec.

## Activated by
`/wiki-ingest`, `/wiki-prd-check`

## Input
- The target spec/PRD page (full content)
- All pages listed in its `sources` field (read each one)
- All pages in vault that link TO this spec (detected via detect-connections)

## Output
A markdown table to be inserted or updated in the spec under `## Evidence Base`:

```markdown
## Evidence Base

| Layer | Source | Finding / Purpose | Links to |
|---|---|---|---|
| Qualitative | [[users/interview-synthesis-checkout-q1]] | 5 users abandon at step 3 | [[users/opportunity-checkout-friction]] |
| Quantitative | [[data/insights/checkout-funnel-q1]] | 23% drop-off at step 3 | [[data/metrics/checkout-conversion]] |
| Design | [[design/references/checkout-v2-figma-v3]] | Variant tested in A/B | [[experiments/checkout-step-removal-ab]] |
| Experiment | [[experiments/checkout-step-removal-ab]] | ✅ Validated — +8% conversion | [[decisions/remove-step-3]] |
| Strategic | [[strategy/q2-okrs]] | OKR: reduce churn 15% | Scope definition |
| Decision | [[decisions/simplify-checkout-flow]] | Core scope decision | [[decisions/remove-step-3]] |
```

## Rules
- Layer types: `Qualitative`, `Quantitative`, `Design`, `Experiment`, `Strategic`, `Decision`, `Competitive`, `Technical`
- Every row must have a valid `[[wikilink]]` in the Source column — no dead links
- If the spec has no sources yet, output an empty table with a comment: `<!-- No sources linked yet — run /wiki-connect -->`
- When updating an existing Evidence Base: append new rows, never remove existing ones
- Experiments show their result status: ✅ Validated / ❌ Invalidated / ⏳ In progress / — Not run

## Example

Input: `wiki/specs/prd-checkout-v2.md` with `sources: ["[[decisions/simplify-checkout-flow]]", "[[data/insights/checkout-funnel-q1]]"]`

Output: table with rows for the decision and the data insight, plus any other pages detected as sources through graph traversal.
