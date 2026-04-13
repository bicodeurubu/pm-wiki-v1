# /wiki-impact [page]

Show everything that would be affected if a given wiki page changes. Use before making an important edit to understand the downstream consequences.

**Agent:** analyst
**Skills:** (none — read-only graph traversal)

---

## When to use
Before changing a decision, updating an OKR, or revising a user persona — when you need to know what else might need updating as a result.

## Usage
```
/wiki-impact wiki/decisions/remove-step-3
/wiki-impact wiki/strategy/q2-okrs
/wiki-impact wiki/users/persona-frequent-shopper
```

## Steps

1. **Read the target page**.

2. **Traverse downstream dependents** — recursively, up to 3 levels.

3. **For each dependent, assess impact severity:**
   - 🔴 **High** — dependent has `status: approved` and the change directly affects its core content
   - 🟡 **Medium** — dependent has `status: review` or the connection is indirect
   - 🟢 **Low** — dependent is a draft or the connection is peripheral

4. **Display impact map:**

```
Impact analysis: wiki/decisions/remove-step-3

If this page changes, the following pages may need updating:

🔴 HIGH IMPACT
  📋 wiki/specs/prd-checkout-v2 [status: review]
     Reason: This decision is listed as a core scope driver in its Evidence Base

🟡 MEDIUM IMPACT
  🧪 wiki/experiments/checkout-step-removal-ab [in progress]
     Reason: Experiment is testing this decision's assumption
  🎨 wiki/design/references/checkout-v2-figma-v3
     Reason: Design is based on the flow defined by this decision

🟢 LOW IMPACT
  📰 wiki/meetings/alignment-2026-04-10
     Reason: Decision was recorded in this meeting note

Total affected: 4 pages (1 high, 2 medium, 1 low)

Recommended action before changing:
1. Review wiki/specs/prd-checkout-v2 — it will need an updated scope section
2. Pause wiki/experiments/checkout-step-removal-ab — its hypothesis may be invalidated
3. Notify design team — Figma artifact may need revision
```

## Output
Printed impact map (not saved unless PM requests). Always ends with recommended actions.

## Notes
- High impact does not mean "do not change" — it means "plan the change carefully"
- The analyst agent never blocks a change — it surfaces information for the PM to decide
