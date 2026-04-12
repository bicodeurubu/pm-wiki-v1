# skill: opportunity-scoring

## Purpose
Score an identified opportunity using a multi-factor framework and return a structured score with rationale — to help prioritize which opportunities to pursue in the OST.

## Activated by
`/wiki-ingest` (when opportunity candidate is detected)

## Input
- The opportunity candidate or existing opportunity page
- Related user research pages (for user value evidence)
- Product strategy page (for alignment evidence)
- Existing experiment results (for validation evidence, if any)

## Output
A scoring block appended to the opportunity page:

```markdown
## Opportunity Score

| Factor | Score (1–5) | Rationale |
|---|---|---|
| User value | 4 | 5 interviews confirmed this pain; users use workarounds today |
| Frequency | 3 | Affects ~40% of sessions based on funnel data |
| Strategic alignment | 5 | Directly maps to Q2 OKR: reduce churn 15% |
| Feasibility | 2 | Requires significant backend changes per engineering estimate |
| Evidence quality | 3 | Mixed: qualitative strong, quantitative limited |

**Total: 17/25**
**Recommendation: High priority — strong alignment and user evidence; plan experiment to de-risk feasibility**

## Scoring notes
- Scores are inputs to PM judgment, not decisions
- Re-score after each major new evidence source (interview, experiment, data pull)
- Last scored: YYYY-MM-DD by [PM name]
```

## Rules
- Always include rationale for each score — a number without explanation is useless
- `Evidence quality` score reflects how much of the evidence is validated (experiments > data > interviews > assumption)
- When evidence is absent for a factor, score it 1 and note: `assumption — no evidence yet`
- Never present the score as a decision — always frame as input to PM judgment
- When re-scoring, preserve the previous score as history (append, do not overwrite)
