# skill: extract-decisions

## Purpose
Read any text (meeting notes, raw ideas, PRD draft, Slack export) and extract decisions — explicit or implicit — formatting each as a candidate `decision-record` page.

## Activated by
`/wiki-ingest`

## Input
- Raw text content from any source

## Output
For each decision found, a structured block:

```
DECISION CANDIDATE:
- title: [short title]
- decision_made: [what was decided]
- options_considered: [list of alternatives that were weighed, if mentioned]
- rationale: [why this option was chosen]
- trade_offs: [what was given up]
- stakeholders: [who was involved or mentioned]
- source: [where this decision was found]
- status: draft
```

If no decisions are found, return: `NO_DECISIONS_DETECTED`

## Rules
- A decision is any choice that was made (or proposed) that affects the product, team, or process.
- Include implicit decisions (e.g., "we agreed to not do X this quarter" is a decision).
- Do NOT include action items or tasks — only choices that have trade-offs.
- When a decision is ambiguous or incomplete, mark it `confidence: uncertain` and flag for PM review.
- Extract all decisions found — do not filter by importance.

## Example

Input: "In today's sync we agreed to remove the guest checkout option for now — too much eng complexity. We'll revisit in Q3. Ana and Bruno were aligned. Pedro wanted to keep it but agreed to the delay."

Output:
```
DECISION CANDIDATE:
- title: Remove guest checkout — Q2
- decision_made: Guest checkout will not be shipped in Q2
- options_considered: Ship guest checkout now, delay to Q3
- rationale: Engineering complexity too high for Q2 timeline
- trade_offs: Some users who don't want to create accounts will be lost
- stakeholders: Ana, Bruno, Pedro
- source: raw/meetings/sync-2026-04-12.md
- status: draft
```
