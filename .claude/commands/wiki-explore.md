# /wiki-explore [topic]

Actively research a topic using web search, then expand or create wiki pages with new findings — connecting them to existing knowledge.

**Agent:** analyst
**Skills:** detect-connections · competitive-teardown (if topic is a competitor)

---

## When to use
When the vault has a gap — a topic is mentioned but under-documented — and you want the agent to actively research it externally and bring findings back into the wiki.

## Usage
```
/wiki-explore "checkout abandonment best practices"
/wiki-explore "Stripe vs Adyen for marketplace payments"
/wiki-explore "Teresa Torres continuous discovery"
```

## Steps

1. **Read existing vault pages** on the topic (if any) — note what's already known and what gaps exist.

2. **Search externally** using available web search tools.

3. **Synthesize findings** — do not copy/paste external content. Extract key insights.

4. **Update or create wiki page:**
   - If page exists: append new section `## New findings — [date]`
   - If no page exists: create a `source` page in the most relevant folder

5. **Run `detect-connections`** — link new content to existing vault pages.

6. **Set `explored: false`** — always. The PM validates external research.

7. **Set `confidence`** based on source quality:
   - `high` — peer-reviewed, official documentation, primary sources
   - `medium` — reputable publications, practitioner blogs
   - `low` — opinion pieces, single-source claims
   - `uncertain` — conflicting sources

8. **Git commit:**
   ```
   git commit -m "feat(wiki): explore [topic] — [N] sources added"
   ```

## Output
Updated or new wiki pages with external research integrated and connected to the graph.

## Notes
- Always include `## Counter-arguments` and `## Data gaps` sections (bias check)
- Never present external research as fact — always maintain confidence rating
- When research contradicts existing vault content, flag the contradiction explicitly
