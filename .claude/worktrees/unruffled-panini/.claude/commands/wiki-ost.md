# /wiki-ost [outcome]

Generate the Opportunity Solution Tree for a given outcome. The OST is derived from the existing connection graph — it reflects what is already known, not what should be done.

**Agent:** analyst
**Skills:** generate-ost

---

## When to use
Before planning sessions, quarterly reviews, or when you want to see the full picture of how your discovery work maps to a product outcome. Also useful for identifying gaps in the OST.

## Usage
```
/wiki-ost wiki/strategy/q2-okrs
/wiki-ost "reduce checkout abandonment"    # searches for matching outcome page
/wiki-ost                                  # generates OST for all outcomes found
```

## Steps

1. **Identify the target outcome** — an OKR or product goal page in `wiki/strategy/`.

2. **Run `generate-ost`** skill — traverses the graph and builds the diagram.

3. **Save output** to `wiki/product/ost-[outcome-slug]-[date].md`.

4. **Display the Mermaid diagram and summary table.**

5. **Always output the Gaps section** — this is one of the most valuable outputs.

6. **Git commit:**
   ```
   git commit -m "feat(wiki): generate OST for [outcome] — [date]"
   ```

## Output
- Mermaid diagram saved to `wiki/product/`
- Printed gaps analysis
- Printed summary table (counts of nodes at each OST level)

## Notes
- The OST is a snapshot — regenerate it as the vault grows
- The diagram only shows what EXISTS in the vault — gaps are shown separately
- OST generation never creates or modifies wiki pages (except the output diagram file)
- Multiple OSTs can coexist (one per outcome, one per quarter)
