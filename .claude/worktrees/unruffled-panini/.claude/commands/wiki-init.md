# /wiki-init

Initialize a new Wiki-PM vault. Creates the complete folder structure, confirms all templates and instruction files are in place, and makes the first git commit.

**Agent:** librarian
**Skills:** validate-frontmatter

---

## When to use
Run once, after cloning the repository into a new product vault. Do not run on an existing vault — it will not overwrite content, but it will create missing folders and flag missing files.

## Steps

1. **Read CLAUDE.md** — confirm schema is present and parseable.

2. **Create folder structure** — ensure all required folders exist:
   ```
   raw/clippings, raw/interviews, raw/data, raw/competitor, raw/ideas
   wiki/strategy, wiki/market, wiki/users, wiki/product, wiki/specs
   wiki/decisions, wiki/data/insights, wiki/data/metrics
   wiki/design/references, wiki/design/diagrams
   wiki/sprints, wiki/meetings, wiki/experiments
   templates/, .claude/agents/base, .claude/agents/custom
   .claude/skills/base, .claude/skills/custom, .claude/commands
   ```

3. **Confirm instruction files** — check that these files exist:
   - `CLAUDE.md`
   - `.claude/AGENTS.md`, `.claude/SKILLS.md`
   - `agents/base/librarian.md`, `agents/base/analyst.md`
   - `agents/_agent-template.md`, `skills/_skill-template.md`
   - All 11 base skills, all 13 commands, all 16 templates

4. **Confirm wiki seed files** — check that `wiki/index.md` and `wiki/log.md` exist. Create them if missing.

5. **Run validate-frontmatter** on any existing wiki pages (if vault is not empty).

6. **Report** — output a summary:
   ```
   ✅ Wiki-PM initialized
   Folders: 20/20 present
   Instruction files: 42/42 present
   Wiki pages found: 0 (empty vault — ready for first ingest)
   ```

7. **Git commit:**
   ```
   git add .
   git commit -m "chore: initialize wiki-pm vault"
   ```

## Output
Console report + git commit. No wiki pages are created.
