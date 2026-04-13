# /wiki-prd-check [spec-page]

Validate a PRD or feature brief against the completeness rubric. Returns a structured gap report with specific next actions.

**Agent:** analyst
**Skills:** prd-review · build-evidence-base

---

## When to use
Before moving a spec from `draft` to `review`, or before a product review meeting, or when a PM wants a second opinion on a spec's readiness.

## Usage
```
/wiki-prd-check wiki/specs/prd-checkout-v2
/wiki-prd-check wiki/specs/spec-guest-mode
```

## Steps

1. **Read the target spec** in full.

2. **Read all pages in its `sources` field.**

3. **Run `build-evidence-base`** — generate or update the Evidence Base table.

4. **Run `prd-review`** — score against the rubric and identify gaps.

5. **Check OST connections:**
   - Is there a linked outcome/OKR?
   - Is there a linked opportunity?
   - Are experiments linked (even if planned)?
   - Is there a design reference?

6. **Output the full review report** (see `prd-review` skill for format).

7. **Do NOT change the spec's status** — only the PM does that.

## Output
Printed review report. Not saved unless PM requests:
```
/wiki-prd-check wiki/specs/prd-checkout-v2 --save
```
When `--save` is used, saves report to `wiki/experiments/prd-review-[spec-slug]-[date].md`.
