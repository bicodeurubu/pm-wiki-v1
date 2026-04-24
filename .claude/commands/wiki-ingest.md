# /wiki-ingest

Process all unprocessed files in `raw/` and compile them into structured wiki pages. The primary command for growing the product brain.

**Agent:** librarian
**Skills:** detect-connections · extract-decisions · interview-synthesis · extract-data-insight · competitive-teardown · build-evidence-base · validate-frontmatter · propagate-changes

---

## When to use
Every time you add files to `raw/`. Run after each batch of new material — interviews, meeting notes, data exports, competitor research, ideas, or PRD drafts.

## Syntax

```
/wiki-ingest             → standard mode: process all unprocessed files automatically
/wiki-ingest --refine    → attended mode: pause before writing each page to ask high-priority refinement questions
```

## Steps

1. **Scan `raw/`** — find all files not yet ingested. A file is "not yet ingested" if it has no frontmatter `ingested: true` field OR if it was modified after its last ingest date. Include `raw/inbox/` in the scan.

2. **Apply file format rules** from CLAUDE.md before classifying:
   - `.mp3`, `.mp4`, `.m4a`, `.wav`, `.mov` → **do not process**. Log as `pending_conversion`, notify PM, continue to next file.
   - `.pdf`, `.pptx` → check for image-heavy content before proceeding (see CLAUDE.md for threshold).
   - All other formats → proceed to classification.

3. **For each processable file, classify** using the detection rules in CLAUDE.md:
   - Check subfolder first as context signal (see CLAUDE.md "Role of subfolders")
   - Structured problem/solution → `spec` (draft)
   - Interview notes/transcript → run `interview-synthesis`
   - Analytics data / metrics export → run `extract-data-insight`
   - Competitor info → run `competitive-teardown`
   - Meeting notes → meeting type (discovery / alignment / review)
   - Decision language → run `extract-decisions`
   - Sprint / task list → sprint page
   - Ideas / rough notes → concept stub
   - Ambiguous → create draft with type `concept`, flag for PM review

4. **Create or update wiki page** using the appropriate template from `templates/`. Set `ingest_state` field.

5. **Assess content for gaps** — generate `refinement_questions` entries for any high or medium priority gaps (see CLAUDE.md for criteria). Set `confidence` accordingly.

6. **If `--refine` flag is active** — before writing the page, present `priority: high` questions to the PM and wait for answers. Incorporate answers into the page before saving.

7. **Run `detect-connections`** on the new page — find related vault pages.

8. **Update `sources` and `dependents`** bidirectionally across all connected pages.

9. **If new page is a spec or PRD draft** — run `build-evidence-base` to construct initial Evidence Base table.

10. **Run `validate-frontmatter`** on the new page — flag missing fields.

11. **Mark raw file as ingested** — add `ingested: true` and `ingested_date: YYYY-MM-DD` to frontmatter of the raw file (or append a comment if no frontmatter).

12. **Update `wiki/index.md`**:
    - Append TLDR entry for the new page under the appropriate section
    - Update **⏳ Refinamento Pendente** section if the page has `refinement_questions`
    - Update **⚠️ Pendentes de Conversão** section for any `pending_conversion` files

13. **Run `propagate-changes`** — notify dependents of new source.

14. **Git commit** per file processed:
    ```
    git commit -m "feat(wiki): ingest [filename] as [type]"
    ```

## Output
- N new or updated wiki pages (with `ingest_state` set)
- Updated `wiki/index.md` (content + refinement pending + conversion pending sections)
- Append to `wiki/log.md` with ingest state for each file
- Git commits per ingested file
- PM notification for any `pending_conversion` or `conversion_error` files

## Notes
- Process files one at a time — do not batch into a single commit
- When a raw file is ambiguous, never skip it — create a draft and flag
- If a wiki page already exists for this content (same topic, same source), append — do not duplicate
- Files in `raw/inbox/` receive no subfolder context prior — classify purely from content
