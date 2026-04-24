# Wiki-PM — Master Schema & Agent Instructions

This file governs everything the AI agent does in this vault. Read it entirely before executing any command.

---

## What This System Is

Wiki-PM is a **pre-compiled knowledge graph** for Product Management teams, not a RAG system. Knowledge is compiled once into structured wiki pages, cross-references are pre-built and maintained, and every artifact is connected to its sources and dependents.

The central metaphor: this vault is the **product brain**. Every decision, research finding, design artifact, data insight, and experiment lives here — connected, traceable, and automatically updated when anything changes.

The organizing framework is the **Opportunity Solution Tree (Teresa Torres)**:

```
Outcome (OKR / Product Goal)
  └── Opportunity (user need, discovered via research + data)
        └── Solution (spec / PRD / design)
              └── Experiment (assumption test → validates or refutes solution)
```

---

## Directory Structure

```
raw/                  Your inbox — place files here before ingesting
  inbox/              ← Unclassified drop zone — use when unsure where to put a file
  clippings/          Web articles, market reports
  interviews/         Raw interview notes or transcripts
  data/               Analytics exports, CSV files, dashboard screenshots
  competitor/         Screenshots, product tours, pricing pages
  ideas/              Quick thoughts, Slack notes, voice memos
  LEIA-ME.md          Instructions for using raw/ — do not delete

wiki/                 Agent-maintained compiled knowledge
  index.md            Master index with TLDRs (token efficiency layer)
  log.md              Append-only changelog — never edit manually
  strategy/           OKRs, vision, product principles
  market/             Competitors, segments, trends
  users/              Personas, JTBD, research synthesis, opportunities
  product/            Product as a persistent entity (overview, roadmap state)
  specs/              PRDs, feature briefs, RFCs
  decisions/          Decision records — immutable once approved
  data/               Analytics insights and metric definitions
    insights/         Specific findings from data tools
    metrics/          Metric definitions and baselines
  design/             Design artifact references and embedded diagrams
    references/       Links to Figma, Miro, Excalidraw with context
    diagrams/         Embedded Mermaid and ASCII diagrams
  sprints/            Sprint plans and retrospectives
  meetings/           Meeting notes by type
  experiments/        Hypotheses, A/B tests, validation results

templates/            Starter templates — used by /wiki-init
.claude/              Agent instructions (you are here)
  agents/             Agent persona definitions
  skills/             Reusable behavior modules
  commands/           Workflow command definitions
```

---

## Mandatory Frontmatter Schema

Every wiki page MUST include this frontmatter. The agent validates completeness on every write.

```yaml
---
title: ""
tldr: ""                          # 1-2 sentence summary for index.md scanning
date_created: YYYY-MM-DD
date_modified: YYYY-MM-DD
type: concept | entity | source | spec | decision | experiment | persona | opportunity | data-insight | metric | design-ref | diagram | synthesis | sop
tags: []
product: ""                       # name of this vault's product
quarter: ""                       # e.g. Q2-2026 — for time-boxed artifacts
status: draft | review | approved | archived
stakeholders: []                  # people involved or affected
sources: []                       # [[wikilinks]] to pages that informed this one
dependents: []                    # [[wikilinks]] to pages that this one informs — maintained by agent
needs_review: false               # agent sets true when a source is updated; only PM resets to false
last_source_update: YYYY-MM-DD    # date of most recent source change
confidence: high | medium | low | uncertain
explored: false                   # only the PM sets true after validating content
ingest_state: processed | processed_with_warnings | pending_conversion | conversion_error
refinement_questions: []          # agent populates; PM resolves via /wiki-refine
---
```

### Field rules
- `sources` and `dependents` are ALWAYS bidirectional. If A lists B as a source, B must list A as a dependent.
- `status: approved` on a `type: decision` page makes it immutable. Never overwrite — append only, or create a superseding decision.
- `needs_review: true` is set by the agent whenever a source page is modified. Only the PM resets it to `false`.
- `explored: false` is always set by the agent. Only the PM sets `true`.
- `dependents` are discovered by scanning the entire vault for pages that cite this one. The agent updates this field on every ingest and propagate.
- `ingest_state` is set by the agent during `/wiki-ingest`. Never set manually.
- `refinement_questions` is populated by the agent when it detects gaps in the page content. Each entry follows the structure `{question, context, priority}` where `priority` is `high | medium | low`. The agent only populates `high` and `medium` priority questions automatically. The PM resolves questions via `/wiki-refine`. When the field is empty (`[]`), no refinement is needed.

### When the agent MUST generate `refinement_questions`

| Situation | Priority |
|---|---|
| Spec with no success metrics defined | `high` |
| Feature with ambiguous requirements ("fast", "easy", "simple") | `high` |
| Opportunity with no frequency or impact data | `high` |
| Persona with no identifiable user segment | `medium` |
| Data insight with no source date or origin | `medium` |
| Decision with only one option considered | `medium` |

Example `refinement_questions` entry:
```yaml
refinement_questions:
  - question: "What is the maximum file size for PDF export?"
    context: "Required to define backend constraints in the spec"
    priority: high
  - question: "Are there specific output format requirements (A4, letter, custom)?"
    context: "Impacts UX decisions for the export dialog"
    priority: medium
```

---

## Page Types

| Type | Definition | Primary folder |
|---|---|---|
| `spec` | PRD or feature brief | `wiki/specs/` |
| `decision` | Decision record with options and rationale | `wiki/decisions/` |
| `experiment` | Hypothesis, method, result | `wiki/experiments/` |
| `persona` | Composite user profile | `wiki/users/` |
| `opportunity` | Identified user need in the OST | `wiki/users/` |
| `data-insight` | Specific finding from analytics | `wiki/data/insights/` |
| `metric` | Metric definition and baseline | `wiki/data/metrics/` |
| `design-ref` | Reference to external design artifact | `wiki/design/references/` |
| `diagram` | Embedded diagram (Mermaid, ASCII) | `wiki/design/diagrams/` |
| `concept` | Framework, idea, or topic | `wiki/strategy/` or `wiki/market/` |
| `entity` | Person, company, tool | anywhere relevant |
| `source` | Summary of one raw source | inline with content area |
| `synthesis` | Cross-cutting analysis | anywhere relevant |
| `sop` | Repeatable process or workflow | `wiki/` root |

---

## The Connection Graph — Core Rules

These rules are non-negotiable. They define the product brain.

**Rule 1 — Never create an orphan page.**
Every new page must have at least one `source` or one `dependent`. If no connection exists yet, create a stub in the most likely related page and link there.

**Rule 2 — Bidirectional links always.**
If page A cites page B in its `sources`, you must open page B and add page A to its `dependents`. No exceptions.

**Rule 3 — Evidence Base is mandatory in specs.**
Every `type: spec` page must have an Evidence Base section (see template). The agent builds this automatically during ingest by scanning sources and decisions.

**Rule 4 — Change propagation is automatic.**
When any page is modified, run the propagation logic: find all `dependents`, set `needs_review: true` on each, add a Change Log entry citing the updated source.

**Rule 5 — Data has an expiry.**
Every `data-insight` page has a `valid_until` field. When today's date passes `valid_until`, the agent sets `needs_review: true` on the insight AND on all its dependents.

**Rule 6 — Design artifacts are references, not files.**
Figma, Miro, Excalidraw, and similar files live outside the vault. The wiki stores `design-ref` pages — rich contextual references that capture what the artifact represents, what changed, and what decisions it informs.

**Rule 7 — The OST emerges from the graph.**
The Opportunity Solution Tree is not maintained manually. It is generated by `/wiki-ost` by traversing the connection graph: outcomes → opportunities → solutions → experiments.

---

## RAW Ingestion — File Format Rules

Before classifying content, the agent must handle file formats correctly. Different file types have different processing costs and capabilities.

### File type behavior

| File type | Action | Reason |
|---|---|---|
| `.md`, `.txt` | Process directly | Native format — lowest cost, highest accuracy |
| `.csv`, `.xlsx` | Process directly | Structured data — light and well-supported |
| `.docx` | Process directly | Text extraction works reliably |
| `.pdf` (text-based) | Process directly | Good extraction quality |
| `.pdf` (image-heavy) | Process with warning | Low text content → high token cost, low accuracy |
| `.pptx` | Process with warning | Images in slides will be ignored |
| `.jpg`, `.jpeg`, `.png` | Process with warning | Only useful if LLM has vision capability |
| `.mp3`, `.mp4`, `.m4a`, `.wav`, `.mov` | Do NOT process — request conversion | LLMs cannot process audio/video natively |

### Handling audio and video files

When the agent finds `.mp3`, `.mp4`, `.m4a`, `.wav`, or `.mov` in `raw/`, it must:

1. **Not attempt to process the file directly.**
2. **Inform the PM clearly:**
   ```
   ⚠️ Audio/video file detected: [filename]
   
   This vault cannot process audio or video directly. Please:
   1. Use any transcription tool of your choice (Whisper, Otter.ai, MacWhisper, etc.)
   2. Save the transcript as [filename]-transcript.txt in the same folder
   3. Run /wiki-ingest again — the transcript will be processed automatically
   
   Skipping this file for now and marking as pending_conversion in wiki/log.md.
   ```
3. **Log the file as `pending_conversion`** in `wiki/log.md`.
4. **Continue processing other files** in the queue.

If Whisper CLI is installed (`which whisper` returns a path), the agent MAY attempt automatic transcription as a convenience — but must ask the PM for confirmation first, since transcription can take several minutes.

### Handling image-heavy PDFs and PPTX

When the agent detects that a PDF or PPTX is predominantly images (less than ~30% extractable text):

1. **Process what is accessible** (text layers, slide titles, speaker notes).
2. **Add a warning to the generated page:**
   ```
   > ⚠️ This page was generated from a file with significant image content.
   > Some information may be missing. Review the original file to verify completeness.
   ```
3. **Set `confidence: low`** in the frontmatter.
4. **Add a `refinement_question`** asking the PM to verify the extracted content.

### File size guidelines

These are soft warnings, not hard blocks. The agent should inform the PM when files are large:

| Type | Warning threshold | Risk |
|---|---|---|
| Audio | > 90 minutes | Long transcript, high token cost |
| PDF | > 80 pages | High token cost, possible timeout |
| PPTX | > 40 slides | Image content likely to be lost |
| Video | Any size | Not processable — always request transcript |

### Ingest states

Every file processed from `raw/` must produce one of these four states, logged in `wiki/log.md`:

| State | Meaning | What the agent does |
|---|---|---|
| `processed` | File ingested successfully | Wiki page created, confidence as assessed |
| `processed_with_warnings` | Ingested but with partial content | Wiki page created, `confidence: low`, inline warning added |
| `pending_conversion` | File type cannot be processed yet | No wiki page created; PM notified; logged with instructions |
| `conversion_error` | File is unreadable or corrupted | No wiki page created; PM notified with error detail; logged |

---

## RAW Ingestion — Detection Rules

When processing files in `raw/`, classify by content signals:

| Signal in content | Type created | Destination |
|---|---|---|
| Structured sections (Problem, Solution, Success Metrics) | `spec` → `draft` | `wiki/specs/` |
| Interview questions/answers, user quotes | `source` + updates `persona`/`opportunity` | `wiki/users/` |
| Competitor name + feature/pricing info | `entity` or `source` | `wiki/market/` |
| Metric numbers, funnel data, conversion rates | `data-insight` | `wiki/data/insights/` |
| Decision made, options considered, rationale | `decision` → `draft` | `wiki/decisions/` |
| Meeting agenda/notes, attendees, action items | appropriate meeting type | `wiki/meetings/` |
| Ideas, shower thoughts, rough notes | `concept` stub | `wiki/` relevant area |
| Sprint goals, capacity, task list | `sop` | `wiki/sprints/` |

When content is ambiguous, prefer creating a `draft` page and flagging for PM review rather than classifying incorrectly.

### Role of subfolders in `raw/`

Subfolders in `raw/` are **context signals for the agent**, not mandatory organization. When a file is in a named subfolder, the agent uses that as a strong prior for classification:

- `raw/interviews/` → treat as interview material even if content is ambiguous
- `raw/data/` → treat as analytics/metrics material
- `raw/competitor/` → treat as competitive intelligence
- `raw/clippings/` → treat as external article or market report
- `raw/ideas/` → treat as concept stub

Files in `raw/inbox/` or directly in `raw/` root receive no prior context. The agent classifies based on content alone. Confidence ceiling for inbox files: `medium` (never `high` without clear content signals).

When processing `raw/inbox/`, classification priority order:
1. Filename (e.g., `entrevista-fulano.mp3` → interview)
2. File content and structure
3. File extension alone (weakest signal)

If classification remains ambiguous after all three signals: create a `concept` stub with `confidence: low`, `needs_review: true`, and a `refinement_question` asking the PM to clarify the content type.

---

## Change Log Rule (for specs and decisions)

Every `type: spec` and `type: decision` page maintains a Change Log table:

```markdown
## Change Log

| Date | Change | Triggered by |
|---|---|---|
| YYYY-MM-DD | Description of what changed | [[source-that-caused-it]] |
```

The agent appends to this table — never rewrites it. It is the audit trail.

---

## Quality Standards

- Source summaries: 200–500 words, synthesized not copied
- Concept and spec pages: 500–1500 words with clear lead section
- Trace every claim to a specific source page — no vague attribution
- Flag contradictions between pages with inline warning: `> ⚠️ Contradicts [[other-page]] — review needed`
- Use `[[wikilinks]]` for all internal references; link first occurrence per section only
- Bold key terms on first use per page
- Filenames: kebab-case, lowercase, no spaces
- Date-prefixed filenames for time-bound artifacts: `YYYY-MM-DD-name.md`

---

## Bias Check (inherited from base system)

Every `concept`, `synthesis`, and `source` page must include:

```markdown
## Counter-arguments
[Opposing viewpoints and pushback on the claims in this page]

## Data gaps
[What's missing, what's unknown, what needs more sources]
```

---

## Agent Orchestration

Commands call agents and skills in sequence. The agent reads `.claude/AGENTS.md` and `.claude/SKILLS.md` to understand the full available toolkit before executing any command.

Custom agents and skills added by the PM team to `agents/custom/` and `skills/custom/` are automatically discovered and available to all commands.

---

## Versioning Rules

- Every ingest, update, and propagation commits to Git automatically
- Commit message format: `type(scope): description`
  - Examples: `feat(wiki): ingest checkout-interview as user-interview`
  - `chore(graph): propagate changes from decisions/remove-step-3 to 3 dependents`
  - `fix(lint): repair broken wikilinks in specs/prd-checkout-v2`
- Never use `git push` automatically — the PM controls when to push to remote
- Decision records with `status: approved` are never overwritten — create a new decision that supersedes them

---

## Audio / PDF Conversion Module

Audio and video files require pre-conversion before ingestion. See **RAW Ingestion — File Format Rules** above for the agent's behavior when encountering these files.

**For automatic local conversion (optional):**
If the PM installs Whisper CLI (`pip install openai-whisper`) and/or Pandoc, the agent can detect them via `which whisper` and `which pandoc` and use them automatically. This is never required — manual conversion is always acceptable.

**For future full automation:**
The hooks `wiki-convert`, `convert-audio`, and `convert-pdf` are reserved in `.claude/commands/` and `skills/` for a future module that handles conversion transparently. When implemented, no PM action will be needed for audio/video files.

---

## Cross-Vault Context

The file `context.md` at the vault root lists other vaults that can be referenced for context. The `/wiki-context` command reads the `index.md` of those vaults and injects their TLDRs as read-only context.

Cross-vault content is NEVER copied into this vault — only referenced.

---

## Scaling Strategy

| Pages | Approach |
|---|---|
| 0–300 | File-based; `index.md` TLDR scanning |
| 300–500 | Add local search layer (e.g., `qmd`) |
| 500+ | Consider PostgreSQL/Supabase migration |
