# /wiki-propagate

Scan for recently modified wiki pages and propagate change awareness to all their dependents. Run after any manual edit to a wiki page.

**Agent:** librarian
**Skills:** propagate-changes

---

## When to use
After manually editing any wiki page (not via ingest). For example: PM approves a decision, updates a spec status, or edits an OKR.

## Steps

1. **Find recently modified files** — check git diff or file modification timestamps for changes in `wiki/` since the last propagate run.

2. **For each modified page:**
   - Read its `dependents` field
   - Run `propagate-changes` skill

3. **Check for stale data insights** — scan all `data-insight` pages where `valid_until` < today. For each stale insight:
   - Set `needs_review: true`
   - Run `propagate-changes` to notify dependents

4. **Update `wiki/log.md`** with a propagation summary.

5. **Git commit:**
   ```
   git commit -m "chore(graph): propagate changes — [N] pages updated"
   ```

## Output
- Updated `needs_review` flags and Change Log entries across dependent pages
- `wiki/log.md` propagation summary
- Git commit

## Notes
- Run `/wiki-propagate` regularly — at minimum before any important PM review or planning session
- If you are unsure what changed: run `/wiki-lint` first to see the current health of the graph
