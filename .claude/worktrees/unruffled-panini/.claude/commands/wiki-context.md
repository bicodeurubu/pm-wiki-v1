# /wiki-context [question]

Query other vaults listed in `context.md` for relevant context. Injects their knowledge as read-only background for a question or analysis.

**Agent:** analyst
**Skills:** (none)

---

## When to use
When you need company-level context (OKRs, shared decisions, platform constraints) that lives in another product vault, and you want to reference it without duplicating content.

## Usage
```
/wiki-context "What are the company-level OKRs this quarter?"
/wiki-context "Has the platform team made any decisions about the auth API?"
```

## Steps

1. **Read `context.md`** — get the list of referenced vaults and their descriptions.

2. **For each referenced vault:**
   - Read its `wiki/index.md` (TLDR layer only)
   - Identify entries relevant to the question

3. **For relevant entries only:** read the full pages in the referenced vault.

4. **Synthesize and respond** — clearly marking which vault the information comes from:
   ```
   From [[../company-okrs-vault]]:
   [Answer based on that vault's content]

   From [[../platform-vault]]:
   [Answer based on that vault's content]
   ```

5. **Never copy content** into this vault — only reference it. If the PM wants to formally link something, they should update `context.md` or create a manual link.

## Output
Printed synthesis. Never modifies this vault's pages.

## Notes
- If a referenced vault path doesn't exist or is inaccessible, report it and skip
- Cross-vault reading is read-only — the analyst never writes to other vaults
