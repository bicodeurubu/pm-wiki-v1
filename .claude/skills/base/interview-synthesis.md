# skill: interview-synthesis

## Purpose
Transform raw user interview notes or transcripts into structured wiki content: identified opportunities, updated personas, JTBD statements, and key quotes.

## Activated by
`/wiki-ingest` (when raw file is classified as interview)

## Input
- Raw interview notes or transcript
- Existing persona pages in `wiki/users/` (read for context)
- Existing opportunity pages in `wiki/users/` (read for context)

## Output
Three outputs:

**1. Interview source page** (`wiki/users/interview-[participant-slug]-[date].md`):
- Summary of participant profile
- Key quotes (verbatim, attributed)
- Top 3 insights
- Jobs-to-be-done identified
- Pains and gains surfaced

**2. Updates to persona page** (append — never rewrite):
- New evidence supporting or contradicting existing traits
- New quote added to the persona's `## Quotes` section

**3. Opportunity candidates** (new stub page or append to existing):
```
OPPORTUNITY CANDIDATE:
- title: [short name of the need]
- user_need: [one sentence — what the user is trying to do]
- pain: [what frustrates them today]
- evidence: [[this interview page]]
- frequency: [how often this came up — this interview only, or also in others]
- confidence: low | medium | high
```

## Rules
- Never paraphrase quotes — use exact words from the transcript
- Never merge two different participants into one insight — keep evidence traceable to source
- When an opportunity already exists in the vault, append evidence — do not create a duplicate
- When no persona exists that matches the participant, create a stub persona page
- Jobs-to-be-done format: "When [situation], I want to [motivation], so I can [expected outcome]"

## Example

Raw input: transcript of a checkout usability session with a user who is a frequent online shopper, gets frustrated at multi-step payment flows, often abandons at the "confirm address" step.

Output:
- `wiki/users/interview-frequent-shopper-2026-04-12.md` — full synthesis
- Append to `wiki/users/persona-frequent-shopper.md` — new quote + address step pain
- `wiki/users/opportunity-checkout-friction.md` — new stub or update with this interview as evidence
