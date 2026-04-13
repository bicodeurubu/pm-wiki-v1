# Clipping — Support Cost Industry Benchmarks (2025)
**Sources compiled from:**
- HDI (Help Desk Institute) "Cost Per Ticket" benchmark study, 2025
- Forrester "Total Economic Impact of AI Support" Q3 2025 (paywalled, summary from Forrester blog)
- MetricNet B2B SaaS Support Benchmarking Report, 2024
- Internal Acme support ops cost analysis (Tomás Ferreira, Dec 2025)
**Compiled by:** Tomás Ferreira (CS Ops)
**Date:** 2026-01-20

---

## Industry cost benchmarks

### Cost per support interaction (human-handled)

| Interaction type | Industry benchmark | Source |
|---|---|---|
| L1 (simple, resolved in first contact) | $12–$18 | HDI 2025 |
| L2 (escalated or complex) | $28–$45 | HDI 2025 |
| L3 / Tier 2 (engineering involved) | $80–$150+ | MetricNet 2024 |
| AI/chatbot resolved (fully automated) | $0.50–$2.00 | Forrester Q3 2025 |

**Acme internal estimate (Tomás, Dec 2025):**
- Blended cost per human-handled ticket: ~$21 (includes agent salary, tooling, overhead)
- Chatbot cost per resolved session: ~$0.80 (LLM API cost + infra allocation)
- Cost ratio: human = ~26x chatbot

At our current volumes (~16,000 human-handled interactions/month), total monthly support cost is approximately $336,000/month or ~$4M/year.

---

## Deflection rate financial model

If we improve deflection rate from 40% to 60% (OKR target), what does it mean in dollars?

**Assumptions:**
- Current: 7,240 chatbot sessions/month, 40% deflected = 2,904 bot-resolved, 4,336 human-handled via chatbot path
- Current: 3,218 direct ticket submissions/month (bypass path)
- Total human-handled: ~7,554/month (4,336 escalated + 3,218 direct)
- Plus human-agent hours already accounted in the $16k-handled figure from ticket taxonomy — note the numbers in this report use a different period/definition than Lena's February funnel data. Align before presenting externally.

**At 60% deflection (chatbot path only):**
- Bot-resolved increases to 4,344/month (from 2,904)
- Human escalations via chatbot drop to 2,896/month (from 4,336)
- Reduction in human-handled: ~1,440 interactions/month
- Monthly savings: 1,440 × $21 = ~$30,240/month
- Annual savings: ~$363,000/year

**Caveat:** This assumes direct ticket submissions (bypass path) don't increase. If improving chatbot quality also reduces the bypass path (e.g., by reducing the "informal ban" behavior), savings could be higher. If users who currently bypass start using the bot and then escalate, the model breaks.

---

## Escalation quality cost

Each post-escalation session costs more than a direct L1 ticket because:
1. The escalation itself costs the chatbot session time (avg 11.7 min)
2. The agent then spends ~8-12 min re-collecting context (per Ryan Kim's estimate)
3. Repeat contact rate is 38% after escalation (vs. 14% after bot resolution and 22% industry benchmark)

**Estimated "escalation waste" cost:**
- Context re-collection: if 74% of escalations require re-collection, and each takes ~10 min at $21/hr blended rate...
  - Monthly: 4,336 escalations × 74% × (10 min / 60 min) × $21 = ~$11,200/month in context re-collection alone
  - Annual: ~$134,000/year in wasted agent time due to context handoff failure

This $134k figure is a direct cost of not fixing escalation context handoff. It doesn't include the indirect cost of lower CSAT, repeat contacts, and churn risk.

---

## Language gap cost

From Q1 ticket taxonomy: 1,408 language-barrier escalations in Q1 (extrapolating to ~470/month).

- 470 language escalations/month × $21 blended cost = $9,870/month
- If multilingual support deflected 70% of these: savings of $6,909/month = ~$83,000/year

Additionally: LogisBR (Marcos interview) flagged a 2-day wait for Portuguese-speaking agents. If those delays drive the LATAM NPS of 11 (vs. 38 for North America), and if even 1-2 LATAM accounts churn due to language issues annually, the ARR impact easily exceeds the build cost. TechCorp alone was $68k ARR.

---

## Proactive support ROI estimates (from Forrester)

Forrester's Q3 2025 blog post on proactive AI support (summary, paywalled full report):

> "Organizations that deployed proactive AI support triggers based on behavioral signals reported 22-35% reduction in inbound ticket volume over 6 months. The median ROI at 12 months was 340% when counting both direct cost savings (fewer tickets) and indirect benefits (improved CSAT, lower churn)."

Applied to Acme (rough):
- If proactive triggers reduce inbound volume by 25%: ~1,900 fewer tickets/month
- Savings: 1,900 × $21 = $39,900/month = ~$479,000/year
- These numbers are directional only. Forrester's sample includes more mature implementations.

---

## Key benchmark gaps (industry vs. Acme)

| Metric | Industry best | Industry median | Acme current | Gap to median |
|---|---|---|---|---|
| Bot CSAT | 4.2 | 3.5 | 3.1 | -0.4 pts |
| Deflection rate | 70-75% | 42-48% | 40% | Approx. at median |
| Escalation rate | 25-30% | 52-58% | 60% (broad) | +2-8 pp above median |
| Repeat contact rate (post-escalation) | 15% | 22% | 38% | +16 pp above median |
| KB update lag | <24 hrs | 3-5 days | 2-6 weeks | Far above median |
| Multilingual support | Table stakes | Standard | Not available | Full gap |

Acme is performing at or below median on every metric, and significantly below median on repeat contact rate and KB freshness.

---

## How to use this data

- Use the $134k/year escalation waste figure in the business case for escalation context handoff (PRD 1)
- Use the $83k/year figure + LATAM churn risk for multilingual business case (PRD 2)
- Use Forrester proactive support ROI framing cautiously — good for direction, not precision
- Align cost definitions between Tomás's model and Lena's funnel data before any exec presentation
