# skill: generate-ost

## Purpose
Traverse the connection graph and build a visual Opportunity Solution Tree (OST) for a given outcome, following Teresa Torres' framework. The OST is generated from existing vault content — it reflects what is already known, not what should be done.

## Activated by
`/wiki-ost`

## Input
- Target outcome: an OKR or product goal (path to strategy page or explicit statement)
- Full vault (traversed via index.md → relevant pages)

## Output
A Mermaid diagram saved to `wiki/product/ost-[outcome-slug]-[date].md`:

```markdown
---
title: "OST: [Outcome name] — [Date]"
type: diagram
diagram_type: ost
generated_from: [[strategy/q2-okrs]]
date_created: YYYY-MM-DD
---

## Opportunity Solution Tree
*Generated [date] — reflects current vault state*

\`\`\`mermaid
graph TD
  OKR["🎯 [Outcome / OKR text]"]

  OPP1["💡 [Opportunity 1 name]"]
  OPP2["💡 [Opportunity 2 name]"]

  SOL1["📋 [Spec/PRD name]"]
  SOL2["📋 [Spec/PRD name]"]

  EXP1["🧪 [Experiment name] ✅"]
  EXP2["🧪 [Experiment name] ⏳"]
  EXP3["🧪 [Experiment name] ❌"]

  DATA1["📊 [Data insight name]"]
  DES1["🎨 [Design ref name]"]

  OKR --> OPP1
  OKR --> OPP2
  OPP1 --> SOL1
  OPP2 --> SOL2
  SOL1 --> EXP1
  SOL1 --> EXP2
  OPP1 --> DATA1
  SOL1 --> DES1
\`\`\`

## OST Summary

| Level | Count | Notes |
|---|---|---|
| Outcomes | 1 | [[strategy/q2-okrs]] |
| Opportunities | 2 | [[users/opp-1]], [[users/opp-2]] |
| Solutions | 2 | [[specs/prd-1]], [[specs/prd-2]] |
| Experiments | 3 | 1 validated, 1 in progress, 1 invalidated |
| Data insights | 1 | Supporting opportunity 1 |

## Gaps detected
- [Opportunity 2] has no experiments planned
- [Opportunity 1] has no quantitative data evidence
- [Opportunity 2] has no design artifact linked
```

## Traversal logic

1. Start from the target outcome page
2. Find all pages where `sources` includes the outcome → these are Opportunities
3. For each Opportunity, find all pages where `sources` includes it → these are Solutions
4. For each Solution, find all pages where `sources` includes it → these are Experiments
5. For each Opportunity, find all `data-insight` pages that link to it
6. For each Solution, find all `design-ref` pages that link to it
7. Build diagram from traversed nodes
8. Detect gaps: opportunities with no solutions, solutions with no experiments

## Rules
- Only include nodes that EXIST in the vault — never invent nodes
- Experiment status icons: ✅ validated / ❌ invalidated / ⏳ in progress / — not started
- Gaps section is always generated — it is one of the most valuable outputs
- The OST diagram is read-only output — it does not create or modify any wiki pages
- Always save with date in filename — each generation is a snapshot in time
