# skill: extract-data-insight

## Purpose
Read analytics exports, dashboard screenshots descriptions, or CSV data summaries and extract structured data insights — connecting findings to metrics, opportunities, and specs.

## Activated by
`/wiki-ingest` (when raw file classified as data/analytics)

## Input
- Raw analytics content (CSV, exported report, dashboard screenshot description, SQL result)
- Existing metric pages in `wiki/data/metrics/` (for baseline context)
- Existing opportunity pages in `wiki/users/` (for connection candidates)

## Output
One or more `data-insight` pages in `wiki/data/insights/`:

```markdown
---
title: "Checkout funnel — step 3 drop-off Q1 2026"
tldr: "23% of users who reach step 3 (address confirmation) abandon the checkout — highest drop-off in funnel"
type: data-insight
source_tool: Metabase
source_url: "https://metabase.company.com/question/142"
captured_date: 2026-04-12
valid_until: 2026-07-12
metric: ["[[data/metrics/checkout-conversion-rate]]"]
opportunity: ["[[users/opportunity-checkout-friction]]"]
dependents: []
confidence: high
---

## Finding
23% of users who reach step 3 of the checkout flow (address confirmation) abandon the purchase. This is the highest drop-off point in the funnel, 2x higher than step 1 and step 2.

## Context
- Time period: Q1 2026 (Jan 1 – Mar 31)
- Sample size: 12,450 sessions
- Segment: all registered users, all platforms

## Implications
This finding supports the hypothesis that the address confirmation step creates unnecessary friction. Combined with qualitative evidence from user interviews, it strengthens the case for [[users/opportunity-checkout-friction]].

## Data gaps
- Mobile vs desktop breakdown not available in this export
- New vs returning user segmentation missing
- No cohort data — cannot tell if this has worsened over time

## Counter-arguments
The drop-off could be intentional user behavior (price comparison, saving cart for later) rather than friction. Exit survey data would be needed to confirm.
```

## Rules
- `valid_until` defaults to 90 days from `captured_date` — adjust if data is known to be stable longer
- Always include sample size and time period in the Context section
- Always include Data gaps and Counter-arguments sections
- When a metric page exists for this metric, link it in frontmatter. If not, create a stub metric page.
- When connecting to opportunities: prefer existing opportunity pages over creating new ones
- When data conflicts with existing insights, flag the contradiction: `> ⚠️ Contradicts [[other-insight]] — review needed`
