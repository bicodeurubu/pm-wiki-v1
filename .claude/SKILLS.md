# Skills Registry

All skills available in this vault. Skills are atomic, reusable behaviors called by commands.

## Rule: Skill vs Command

- **Skill** = one thing, done well, reusable anywhere
- **Command** = a workflow that sequences multiple skills toward a goal

## Base Skills

| Skill | File | Called by | What it does |
|---|---|---|---|
| detect-connections | `skills/base/detect-connections.md` | ingest, connect | Scans vault for pages related to new content |
| extract-decisions | `skills/base/extract-decisions.md` | ingest | Pulls decisions from any raw text |
| build-evidence-base | `skills/base/build-evidence-base.md` | ingest, prd-check | Builds the Evidence Base table for specs |
| propagate-changes | `skills/base/propagate-changes.md` | ingest, propagate | Updates dependents when a source changes |
| validate-frontmatter | `skills/base/validate-frontmatter.md` | ingest, lint, init | Checks all required frontmatter fields |
| prd-review | `skills/base/prd-review.md` | prd-check | Validates PRD completeness against rubric |
| interview-synthesis | `skills/base/interview-synthesis.md` | ingest | Converts raw interviews to structured insights |
| competitive-teardown | `skills/base/competitive-teardown.md` | ingest, explore | Structures competitor analysis |
| opportunity-scoring | `skills/base/opportunity-scoring.md` | ingest | Scores opportunities by value, viability, alignment |
| extract-data-insight | `skills/base/extract-data-insight.md` | ingest | Extracts metric findings from analytics exports |
| generate-ost | `skills/base/generate-ost.md` | ost | Builds OST diagram by traversing the graph |

## Custom Skills

Place custom skill files in `skills/custom/`. They are automatically available to all commands.
See `skills/_skill-template.md` for the required format.

| Skill | File | Called by | What it does |
|---|---|---|---|
| *(none yet)* | | | |

## Example custom skills teams add

```
skills/custom/skill-daci-format.md          formats decisions using DACI framework
skills/custom/skill-shape-up-pitch.md       converts opportunity to Shape Up pitch
skills/custom/skill-jira-export.md          formats wiki content for Jira import
skills/custom/skill-weekly-digest.md        generates weekly activity digest
skills/custom/skill-stakeholder-update.md   generates stakeholder update from sprints
```
