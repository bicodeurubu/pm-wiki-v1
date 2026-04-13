# skill: prd-review

## Purpose
Review a PRD or feature brief against a completeness rubric and return a structured gap report with specific, actionable feedback.

## Activated by
`/wiki-prd-check`

## Input
- The target PRD/spec page (full content)
- Its Evidence Base (built or existing)
- Its linked decisions, experiments, and opportunities

## Output
A structured review report:

```markdown
# PRD Review: [spec title]

## Score: [X/10]

## ✅ Present and adequate
- Problem statement: clear, user-centric
- Success metrics: defined with baseline and target
- ...

## ⚠️ Present but weak
- Scope (in/out): listed but no rationale for exclusions
  → Suggestion: explain WHY each out-of-scope item was excluded

## ❌ Missing
- No linked opportunity in OST
  → Action: run /wiki-connect or create opportunity page in wiki/users/
- No experiments planned
  → Action: create at least one experiment in wiki/experiments/ before status → review
- Evidence Base has no quantitative sources
  → Action: add at least one data-insight or metric to Evidence Base

## Connections check
- Linked to outcome (OKR): ✅ [[strategy/q2-okrs]]
- Linked to opportunity: ❌ None found
- Linked to decisions: ✅ [[decisions/simplify-checkout-flow]]
- Linked to design: ⚠️ [[design/references/checkout-v2-figma-v3]] — needs_review: true
- Experiments: ⏳ [[experiments/checkout-step-removal-ab]] — in progress
```

## Rubric (10 points)

| Area | Points | What good looks like |
|---|---|---|
| Problem statement | 1 | User-centric, not solution-centric. Explains who is affected and why it matters |
| Opportunity link | 1 | Linked to at least one `opportunity` page in OST |
| Evidence Base | 2 | Has qualitative + quantitative sources. All rows have valid wikilinks |
| Success metrics | 1 | Specific, measurable, with baseline value and target |
| Scope (in/out) | 1 | Explicit list of what is NOT in scope and why |
| Design reference | 1 | Linked to at least one design-ref page |
| Experiments planned | 1 | At least one experiment linked (can be in planning state) |
| Stakeholders | 1 | Named with roles, not just a list |
| Risk / open questions | 1 | At least 3 risks or open questions documented |

## Rules
- Score is informational — the PM decides whether to proceed regardless of score
- Never block a PRD from moving to `review` status — only flag gaps
- Always end with 3 specific next actions, ordered by priority
