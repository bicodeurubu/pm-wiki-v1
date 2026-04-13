# Acme Inc — OKRs 2026
**Version:** 1.2 (revised after Q4 2025 review)
**Approved by:** Executive team (January 2026 offsite)
**Owner:** Strategy & Operations
**Last updated:** 2026-01-22
**Distribution:** All Acme employees

---

## Company context

Acme is entering 2026 in a position of growth but increasing competitive pressure. We grew ARR 34% in 2025 to $48M. Net Revenue Retention (NRR) was 104% — good but down from 112% in 2024, driven by increased churn in H2 2025. Key themes for 2026:

1. **LATAM expansion** — we've proven the market (12 clients in our pilot cohort), now we need to scale it to 50+ clients without proportionally scaling headcount
2. **Support efficiency** — our support cost as % of revenue grew from 8% to 11% in 2025. This is unsustainable at scale. We need AI to do more.
3. **Retention** — NRR must recover to >110%. H2 2025 churn was concentrated in enterprise accounts and correlated with poor support experiences.
4. **Product velocity** — we shipped 8 major features in 2025 (up from 5 in 2024). We need to maintain velocity while improving quality.

---

## Company OKRs 2026

### O1: Become the go-to project management platform for LATAM enterprise teams

**KR1.1:** Close 50 new LATAM clients by December 31, 2026 (current: 12)
**KR1.2:** LATAM accounts represent ≥15% of total ARR by December 31, 2026 (current: ~3%)
**KR1.3:** LATAM client NRR ≥ 100% by end of 2026 (current: unmeasurable — cohort too new)
**KR1.4:** Average LATAM onboarding time ≤ 2 weeks (current: 4-5 weeks, driven by language friction)

*Notes from strategy offsite: LATAM expansion is the #1 growth initiative. The exec team sees Brazil and Mexico as the highest-priority markets. The 4-5 week onboarding time is flagged as a blocker — root cause is language friction in both product documentation and support. Marketing already localizing the website. Product and Support must localize the chatbot and help center.*

---

### O2: Reduce the cost and improve the quality of customer support

**KR2.1:** Reduce support cost as % of revenue from 11% to 7% by December 31, 2026
**KR2.2:** Increase chatbot deflection rate from 40% to 60% by Q3 2026
**KR2.3:** Reduce chatbot escalation rate from 60% to <35% by Q3 2026
**KR2.4:** Achieve chatbot CSAT ≥ 3.8 by Q4 2026 (current: 3.1)
**KR2.5:** Reduce knowledge base update lag from current 2-6 weeks to <48 hours by Q2 2026

*Notes: KR2.2 and KR2.3 are in tension — it's possible to improve deflection by being more aggressive with "bot resolution" tagging without actually resolving more issues. PM team is responsible for defining a consistent methodology. KR2.4 is the quality check that prevents gaming KR2.2 and KR2.3.*

*KR2.5 (KB lag) was added after the TechCorp churn postmortem. The 2-6 week lag is now a documented churn driver, not just a quality complaint.*

---

### O3: Achieve NRR > 110% through retention and expansion

**KR3.1:** Gross revenue churn ≤ 5% (current: 7.2%)
**KR3.2:** Enterprise account (200+ seats) NPS ≥ 25 by Q4 2026 (current: 18)
**KR3.3:** Support experience NPS ≥ 40 by Q4 2026 (current: 32)
**KR3.4:** Identify and implement at least 3 expansion revenue motions by Q3 2026

*Notes: Enterprise NPS of 18 is alarming. This segment has the most complex support needs and suffers disproportionately from escalation quality. Support improvement (O2) is a direct lever for O3. These OKRs are linked.*

---

### O4: Maintain product velocity while improving engineering quality

**KR4.1:** Ship 10+ customer-facing features in 2026 (2025 baseline: 8)
**KR4.2:** P1 bug rate < 2 per quarter (2025: averaged 4.5/quarter)
**KR4.3:** Zero incidents resulting from chatbot outdated KB causing customer-visible errors in production (currently ~3-4/month)

*Note on KR4.3: This is an unusual KR — it's tracking a support quality metric in an engineering OKR. This was added because the "chatbot giving wrong instructions that makes things worse" pattern (TechCorp incident, multiple interviews) is partly an engineering + knowledge management failure, not just a product issue. Engineering team agreed to own this.*

---

## Product team OKRs 2026 (Support Chatbot)

These are the product-level OKRs that contribute to Company O2 and O3.

### PO1: Make the chatbot capable enough to earn user trust

**PKR1.1:** Deflection rate 60% by Q3 2026 (supports KR2.2)
**PKR1.2:** Chatbot CSAT 3.8 by Q4 2026 (supports KR2.4)
**PKR1.3:** Post-escalation CSAT ≥ chatbot CSAT (currently 2.8 < 3.1 — escalation actively hurts)
**PKR1.4:** Repeat contact rate after escalation ≤ 20% by Q4 2026 (current: 38%)

### PO2: Enable support in Portuguese and Spanish for LATAM expansion

**PKR2.1:** Portuguese and Spanish chatbot support live by Q2 2026
**PKR2.2:** Non-English escalation rate ≤ 60% by Q3 2026 (current: 94% for PT/ES)
**PKR2.3:** LATAM-specific support CSAT ≥ 3.5 by Q4 2026 (no current baseline)

### PO3: Close the knowledge freshness gap

**PKR3.1:** KB update pipeline delivers new content within 48 hrs of product change by Q2 2026 (supports KR2.5)
**PKR3.2:** Zero "incorrect escalation" tickets in QA tracking attributable to KB staleness for features >30 days old by Q3 2026

---

## What's NOT in OKRs (explicitly excluded)

The following were discussed at the offsite and deliberately excluded:

- **Agent headcount reduction target:** Intentionally not included. We want to improve cost efficiency through better AI, not set a headcount reduction goal that creates perverse incentives. CS team should grow with the business, just at a slower rate.
- **Chatbot "sessions handled" volume target:** Volume without quality is misleading (see tension note on KR2.2/2.3). We track quality metrics, not raw volume.
- **Revenue from support upsell:** Discussed but deferred to 2027. Not appropriate until support quality is at parity.

---

## Dependencies and risks

| OKR | Key dependency | Risk if dependency fails |
|---|---|---|
| O1 (LATAM) | Multilingual chatbot (PO2) | Onboarding time stays at 4-5 weeks; LATAM growth stalls |
| O2 (support cost) | Escalation context fix (PKR1.3) + KB freshness (PO3) | Cost target missed; NRR suffers |
| O3 (NRR) | Enterprise NPS improvement (requires O2 progress) | Churn continues at 7%+ |
| O4 KR4.3 | KB update pipeline (PO3) | Continues creating production-visible errors from outdated chatbot |

*Critical path: PO2 (multilingual) and PKR1.3 (post-escalation CSAT) are the most direct levers for O1 and O3 respectively. These should be the highest-priority product investments in H1 2026.*
