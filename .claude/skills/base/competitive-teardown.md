# skill: competitive-teardown

## Purpose
Structure raw competitor information (screenshots, notes, pricing pages, product tours) into a standardized competitive analysis page in the wiki.

## Activated by
`/wiki-ingest` (when raw file classified as competitor), `/wiki-explore`

## Input
- Raw competitor content (notes, screenshots description, pricing info, feature list)
- Existing competitor page for this company (if any — append, do not rewrite)
- Product overview from `wiki/product/` (for positioning comparison)

## Output
A `wiki/market/competitor-[company-slug].md` page (or update to existing):

```markdown
## Positioning
[How they position themselves — their core promise]

## Target customer
[Who they primarily serve]

## Key features
[What they do well — specific, not generic]

## Pricing
[Model and tiers if known — with date captured]

## Weaknesses
[Where they fall short based on evidence — not opinion]

## Threats to our product
[Where they directly compete with us, and how strong the overlap is]

## Opportunities for differentiation
[Where we can win or where they are not focused]

## Evidence
[Links to raw sources — screenshots, reviews, trial notes]
```

## Rules
- Every weakness and threat must be supported by evidence — no speculation presented as fact
- If pricing is not confirmed, mark as `estimated` with date
- When a feature comparison is uncertain, use `unconfirmed` rather than omitting
- Differentiation opportunities must relate to our product's known opportunities (link to `wiki/users/opportunity-*.md`)
- Include a `## Counter-arguments` section: where is this competitor actually strong despite our wishful thinking?
- Freshness: add `captured_date` and `valid_until` (default 90 days) to frontmatter
