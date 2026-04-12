# /wiki-ingest

Process all unprocessed files in `raw/` and compile them into structured wiki pages. The primary command for growing the product brain.

**Agent:** librarian
**Skills:** detect-connections · extract-decisions · interview-synthesis · extract-data-insight · competitive-teardown · build-evidence-base · validate-frontmatter · propagate-changes

---

## When to use
Every time you add files to `raw/`. Run after each batch of new material — interviews, meeting notes, data exports, competitor research, ideas, or PRD drafts.

## Steps

1. **Scan `raw/`** — find all files not yet ingested. A file is "not yet ingested" if it has no frontmatter `ingested: true` field OR if it was modified after its last ingest date.

2. **For each file, classify** using the detection rules in CLAUDE.md:
   - Structured problem/solution → `spec` (draft)
   - Interview notes/transcript → run `interview-synthesis`
   - Analytics data / metrics export → run `extract-data-insight`
   - Competitor info → run `competitive-teardown`
   - Meeting notes → meeting type (discovery / alignment / review)
   - Decision language → run `extract-decisions`
   - Sprint / task list → sprint page
   - Ideas / rough notes → concept stub
   - Ambiguous → create draft with type `concept`, flag for PM review

3. **Create or update wiki page** using the appropriate template from `templates/`.

4. **Run `detect-connections`** on the new page — find related vault pages.

5. **Update `sources` and `dependents`** bidirectionally across all connected pages.

6. **If new page is a spec or PRD draft** — run `build-evidence-base` to construct initial Evidence Base table.

7. **Run `validate-frontmatter`** on the new page — flag missing fields.

8. **Mark raw file as ingested** — add `ingested: true` and `ingested_date: YYYY-MM-DD` to frontmatter of the raw file (or append a comment if no frontmatter).

9. **Update `wiki/index.md`** — append TLDR entry for the new page.

10. **Run `propagate-changes`** — notify dependents of new source.

11. **Git commit** per file processed:
    ```
    git commit -m "feat(wiki): ingest [filename] as [type]"
    ```

## Output
- N new or updated wiki pages
- Updated `wiki/index.md`
- Append to `wiki/log.md`
- Git commits per ingested file

## Notes
- Process files one at a time — do not batch into a single commit
- When a raw file is ambiguous, never skip it — create a draft and flag
- If a wiki page already exists for this content (same topic, same source), append — do not duplicate
