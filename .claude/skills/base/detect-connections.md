# skill: detect-connections

## Purpose
For a given piece of content (new file or updated page), scan the entire vault and identify existing pages that are related — by topic, entity, product area, or explicit mention. Returns a list of connection candidates with relationship type.

## Activated by
`/wiki-ingest`, `/wiki-connect`

## Input
- The content of the new or updated file
- The current `wiki/index.md` (TLDR layer — read this first for efficiency)
- Full text of candidate pages (read only those flagged by TLDR scan)

## Output
A structured list of detected connections:

```
SOURCES (pages that informed this content):
- [[wiki/decisions/page-name]] — reason for connection
- [[wiki/users/page-name]] — reason for connection

DEPENDENTS (pages that this content should inform):
- [[wiki/specs/page-name]] — reason for connection
```

This output is consumed by the ingest command to update `sources` and `dependents` frontmatter bidirectionally.

## Rules
- Read `wiki/index.md` TLDRs first. Only open full pages if the TLDR suggests relevance.
- A connection exists if: same entity is mentioned, same product area, same user need, same metric, or same decision is referenced.
- Minimum threshold: a connection must be explicit or strongly implied — not just topically adjacent.
- When uncertain about a connection: include it with `confidence: low` annotation and let the PM decide.
- Never invent connections. If nothing relevant exists in the vault, return empty list.

## Example

Input: raw file describing a user interview where a user abandons a checkout flow at the payment step.

Output:
```
SOURCES:
- [[wiki/users/persona-power-user]] — interview subject matches this persona

DEPENDENTS:
- [[wiki/specs/prd-checkout-v2]] — mentions checkout friction as core problem
- [[wiki/data/insights/checkout-funnel-q1]] — same funnel drop-off point
- [[wiki/users/opportunity-checkout-friction]] — this interview is direct evidence
```
