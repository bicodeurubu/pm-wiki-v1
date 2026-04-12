# skill: validate-frontmatter

## Purpose
Check that a wiki page has all required frontmatter fields present and correctly formatted. Return a validation report.

## Activated by
`/wiki-ingest`, `/wiki-lint`, `/wiki-init`

## Input
- One or more wiki page files to validate

## Output
For each page, one of:

**PASS:**
```
✅ wiki/specs/prd-checkout-v2.md — all fields present
```

**FAIL with details:**
```
❌ wiki/specs/prd-checkout-v2.md — missing fields:
   - quarter: empty
   - dependents: empty (run /wiki-connect to populate)
   - tldr: empty <!-- NEEDS PM REVIEW -->
```

Aggregate summary at the end:
```
Validated: 12 pages | Pass: 10 | Fail: 2
```

## Required fields by type

**All types:**
`title`, `tldr`, `date_created`, `date_modified`, `type`, `tags`, `product`, `status`, `sources`, `dependents`, `needs_review`, `confidence`, `explored`

**Additional for `spec`:**
`quarter`, `stakeholders`

**Additional for `decision`:**
`quarter`, `stakeholders`

**Additional for `experiment`:**
`opportunity`, `solution`, `metric`, `test_method`, `result`

**Additional for `data-insight`:**
`source_tool`, `captured_date`, `valid_until`, `metric`

**Additional for `design-ref`:**
`tool`, `url`, `version`

## Rules
- `tldr` must not be empty for any published page (status ≠ draft)
- `sources` and `dependents` may be empty for newly ingested draft pages — flag but do not fail
- `explored: true` set by agent = always flag as error (only PM sets this)
- `type` must be one of the valid types listed in CLAUDE.md
- Date fields must be in YYYY-MM-DD format
