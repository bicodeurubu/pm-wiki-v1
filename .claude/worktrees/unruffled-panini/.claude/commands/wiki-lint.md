# /wiki-lint

Health check the entire vault — find structural problems, broken connections, stale content, and PM-specific gaps.

**Agent:** librarian
**Skills:** validate-frontmatter

---

## When to use
Periodically — weekly or before planning sessions. Also useful after a large batch of ingests to verify graph integrity.

## Steps

1. **Frontmatter validation** — run `validate-frontmatter` on all wiki pages. Flag missing or malformed fields.

2. **Broken wikilinks** — scan all pages for `[[links]]` that point to non-existent pages. For each broken link:
   - If the page should exist: create a stub
   - If the link is a typo: flag for PM correction

3. **Orphan pages** — find pages with no inbound links (not cited by any other page). Flag for connection or archival.

4. **Stale data insights** — find all `data-insight` pages where `valid_until` < today. Flag their dependents.

5. **needs_review backlog** — list all pages with `needs_review: true`, sorted by how long they've been flagged.

6. **OST gaps (PM-specific)** — check for:
   - Specs with no linked opportunity
   - Opportunities with no data evidence
   - Opportunities with no experiments planned or run
   - Decisions with no sources
   - Experiments with no linked metric

7. **Duplicate detection** — find pages with very similar titles that might be duplicates.

8. **Output lint report:**

```markdown
# Wiki Lint Report — YYYY-MM-DD

## Summary
Pages scanned: 47
Issues found: 12 (3 critical, 5 moderate, 4 low)

## 🔴 Critical
- 2 broken wikilinks in wiki/specs/prd-checkout-v2.md
  → [[decisions/phased-rollout]] does not exist — create stub or fix link
- wiki/decisions/payment-provider-choice has no sources
  → A decision without evidence is an assumption — add sources

## 🟡 Moderate
- 5 pages with needs_review: true for more than 14 days
  → wiki/specs/prd-checkout-v2 (flagged 18 days ago)
  → ...

## 🟢 Low
- 4 orphan pages (no inbound links)
  → wiki/meetings/review-2026-03-01 — consider archiving or linking

## OST Gaps
- wiki/users/opportunity-mobile-ux: no experiments planned
- wiki/specs/prd-notifications: no design reference linked

## Auto-fixed
- Created 1 stub page for broken link
- No duplicates found
```

9. **Git commit** for any auto-fixes:
   ```
   git commit -m "fix(lint): repair [N] broken links, create [N] stubs"
   ```
