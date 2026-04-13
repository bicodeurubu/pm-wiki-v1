# /wiki-trace [page]

Show the complete evidence chain for a given wiki page — everything that informed it (upstream) and everything it informs (downstream).

**Agent:** analyst
**Skills:** (none — read-only graph traversal)

---

## When to use
Before a review meeting, stakeholder presentation, or when someone asks "why did we decide this?" or "what is this spec based on?"

## Usage
```
/wiki-trace wiki/specs/prd-checkout-v2
/wiki-trace wiki/decisions/remove-step-3
/wiki-trace wiki/users/opportunity-checkout-friction
```

## Steps

1. **Read the target page** — note its `sources` and `dependents`.

2. **Traverse upstream** (sources): for each source, read it and collect its sources too. Go up to 3 levels.

3. **Traverse downstream** (dependents): for each dependent, read it and collect its dependents too. Go up to 3 levels.

4. **Build and display the trace tree:**

```
📋 wiki/specs/prd-checkout-v2 [status: review]
│
├── 📥 INFORMED BY (sources)
│   ├── ⚖️  wiki/decisions/simplify-checkout-flow [approved]
│   │   └── 📊 wiki/data/insights/checkout-funnel-q1 [confidence: high]
│   │   └── 👤 wiki/users/interview-synthesis-checkout-q1
│   ├── 🎯 wiki/strategy/q2-okrs [approved]
│   └── 👤 wiki/users/opportunity-checkout-friction
│       ├── 👤 wiki/users/interview-shopper-2026-03-15
│       └── 📊 wiki/data/insights/checkout-funnel-q1
│
└── 📤 INFORMS (dependents)
    ├── 🧪 wiki/experiments/checkout-step-removal-ab [✅ validated]
    ├── 🧪 wiki/experiments/checkout-guest-mode-smoke [❌ invalidated]
    └── 🎨 wiki/design/references/checkout-v2-figma-v3

⚠️  Flags:
- wiki/design/references/checkout-v2-figma-v3 has needs_review: true
- wiki/users/opportunity-checkout-friction has explored: false
```

## Output
Printed trace tree (not saved to a file unless PM requests it).
Flags section always shown — surfaces pages needing PM attention.

## Notes
- Icons: 📋 spec · ⚖️ decision · 📊 data-insight · 👤 user/persona/opportunity · 🎯 strategy · 🧪 experiment · 🎨 design · 📰 source
- If a source or dependent page has `needs_review: true`, flag it prominently
- Circular references (A → B → A): detect and break the loop with a note
