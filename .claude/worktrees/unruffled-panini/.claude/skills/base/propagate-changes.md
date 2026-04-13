# skill: propagate-changes

## Purpose
When a wiki page is created or modified, find all its dependents and notify them of the change — setting `needs_review: true`, appending to their Change Log, and updating `last_source_update`.

## Activated by
`/wiki-ingest`, `/wiki-propagate`

## Input
- The page that was created or modified (path + title)
- The date of modification
- The nature of the change (brief description)

## Output
For each dependent page found:
1. Set `needs_review: true` in frontmatter
2. Set `last_source_update: YYYY-MM-DD` in frontmatter
3. Append to the page's `## Change Log` table:

```markdown
| YYYY-MM-DD | Source updated: [[changed-page]] — [nature of change] | [[changed-page]] |
```

4. Update `wiki/log.md` with one line per affected page:
```
YYYY-MM-DD | propagate | [[changed-page]] → [[dependent-1]], [[dependent-2]] | [nature of change]
```

5. Stage and commit all modified files:
```
git add .
git commit -m "chore(graph): propagate changes from [page] to [N] dependents"
```

## Rules
- Traverse ALL dependents, including indirect ones (dependents of dependents), up to 2 levels deep. Flag 3rd-level dependencies for PM review rather than auto-updating.
- Never set `needs_review: false` — only the PM does this.
- Never modify the content of dependent pages — only frontmatter and Change Log.
- If a dependent has `status: approved` and `type: decision`: do not modify it. Instead, create a note in `wiki/log.md` flagging the potential impact.
- Always commit after propagation — the git history is the audit trail.

## Example

Page modified: `wiki/decisions/remove-step-3.md` (status changed to `approved`)

Dependents found: `wiki/specs/prd-checkout-v2.md`, `wiki/experiments/checkout-step-removal-ab.md`

Result:
- Both pages get `needs_review: true` and Change Log entry
- `wiki/log.md` gets: `2026-04-12 | propagate | [[decisions/remove-step-3]] → [[specs/prd-checkout-v2]], [[experiments/checkout-step-removal-ab]] | decision approved`
- Git commit: `chore(graph): propagate changes from decisions/remove-step-3 to 2 dependents`
