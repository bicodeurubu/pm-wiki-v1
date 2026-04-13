# /wiki-query [question]

Answer a question about the product by synthesizing information from across the vault. Saves the answer as a wiki page for future reference.

**Agent:** analyst
**Skills:** (none — read-only synthesis)

---

## When to use
When you need to understand the current state of the product, retrieve a decision rationale, or synthesize evidence across multiple pages.

## Usage
```
/wiki-query "What is the evidence for simplifying the checkout flow?"
/wiki-query "What experiments have we run this quarter and what were the results?"
/wiki-query "What does our persona data say about mobile users?"
```

## Steps

1. **Read `wiki/index.md`** — scan TLDRs to identify relevant pages.

2. **Read full content** of relevant pages only (do not read the entire vault).

3. **Synthesize answer** with explicit citations to `[[wiki pages]]`.

4. **Flag confidence** — if evidence is thin or contradictory, say so explicitly.

5. **Save output** to `wiki/experiments/query-[question-slug]-[date].md` with type `synthesis`.

6. **Git commit:**
   ```
   git commit -m "feat(wiki): save query output — [question-slug]"
   ```

## Output format

```markdown
# Query: [Question]
*Answered: YYYY-MM-DD*

## Answer
[Synthesized response with [[wikilinks]] to all sources cited]

## Sources used
- [[page-1]] — [why it was relevant]
- [[page-2]] — [why it was relevant]

## Confidence: [high | medium | low]
[Explanation of confidence level — what's well-evidenced vs assumed]

## Gaps
[What would make this answer more complete or certain]
```
