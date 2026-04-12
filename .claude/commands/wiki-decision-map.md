# /wiki-decision-map [area]

Generate a visual map of all decisions in a product area — their status, what they're based on, and what they affect.

**Agent:** analyst
**Skills:** (none — read-only graph traversal)

---

## When to use
Before planning sessions, sprint kickoffs, or when onboarding a new PM to the product area. Shows the landscape of past decisions and their current validity.

## Usage
```
/wiki-decision-map checkout        # decisions tagged with 'checkout'
/wiki-decision-map                 # all decisions in the vault
/wiki-decision-map Q2-2026         # decisions from a specific quarter
```

## Steps

1. **Find all `type: decision` pages** matching the filter (tag, quarter, or all).

2. **For each decision, read:** status, stakeholders, sources, dependents, needs_review.

3. **Build the decision map:**

```
Decision Map — checkout — 2026-04-12

┌─────────────────────────────────────────────────────────┐
│ APPROVED (immutable)                                    │
├─────────────────────────────────────────────────────────┤
│ ✅ decisions/simplify-checkout-flow                     │
│    Based on: 3 interviews + funnel data Q1              │
│    Affects: prd-checkout-v2, experiment-step-removal    │
│    Stakeholders: Ana, Bruno                             │
│                                                         │
│ ✅ decisions/remove-step-3                              │
│    Based on: simplify-checkout-flow + AB test result    │
│    Affects: prd-checkout-v2 ⚠️ needs_review             │
│    Stakeholders: Ana, Engineering Lead                  │
├─────────────────────────────────────────────────────────┤
│ REVIEW                                                  │
├─────────────────────────────────────────────────────────┤
│ 🔄 decisions/guest-checkout-delay                       │
│    Proposed: delay guest checkout to Q3                 │
│    Based on: team sync 2026-04-10                       │
│    Affects: prd-checkout-v2                             │
├─────────────────────────────────────────────────────────┤
│ DRAFT                                                   │
├─────────────────────────────────────────────────────────┤
│ 📝 decisions/payment-provider-choice [needs_review]     │
│    Draft: choose between Stripe and Adyen               │
│    Based on: RFP results — source outdated              │
└─────────────────────────────────────────────────────────┘

Flags:
⚠️  2 decisions have dependents with needs_review: true
⚠️  1 decision source is past valid_until date
```

## Output
Printed decision map. Not saved unless PM requests `--save`.
