# /wiki-connect [page]

Force a full connection re-scan for a specific page — or for all pages if no argument given. Rebuilds `sources` and `dependents` bidirectionally.

**Agent:** librarian
**Skills:** detect-connections · propagate-changes

---

## When to use
- After manually creating a wiki page (bypassing ingest)
- When you suspect connections are missing or stale
- After adding many new pages and wanting to ensure the graph is fully connected
- Periodically as maintenance (e.g., weekly)

## Usage
```
/wiki-connect wiki/specs/prd-checkout-v2    # single page
/wiki-connect                                # full vault scan
```

## Steps

1. **For the target page (or all pages if no argument):**

2. **Read the page content.**

3. **Run `detect-connections`** — identify source and dependent candidates.

4. **Compare** candidates against current `sources` and `dependents` frontmatter.

5. **Update missing connections** — add to `sources` and `dependents` fields.

6. **Update the other side** — for each new connection, open the related page and add the reverse link.

7. **Run `propagate-changes`** for any newly connected sources (to ensure `needs_review` flows correctly).

8. **Git commit:**
   ```
   git commit -m "chore(graph): rebuild connections for [page or 'all pages']"
   ```

## Output
- Updated frontmatter on all affected pages
- Connection report:
  ```
  wiki/specs/prd-checkout-v2
    New sources added: 2 ([[decisions/...]], [[users/...]])
    New dependents found: 0
    Already connected: 3
  ```
