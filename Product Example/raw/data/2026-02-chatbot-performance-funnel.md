# Chatbot Performance Funnel — February 2026
**Prepared by:** Lena Park (Data Engineer)
**Requested by:** Priya Nair (PM) for Q1 planning
**Data source:** Acme internal analytics (Amplitude + Zendesk + internal chatbot logs)
**Coverage:** February 1–28, 2026 (full month, clean data)
**Report generated:** 2026-03-05

---

## Executive summary numbers (February 2026)

| Metric | Value | Target (2026) | Status |
|---|---|---|---|
| Total chatbot sessions initiated | 7,240 | — | — |
| Deflection rate (bot resolved, no human needed) | 40.1% | 60% | 🔴 Far below target |
| Escalation rate | 59.9% | <35% | 🔴 Far above target |
| Chatbot CSAT (1-5 scale) | 3.1 | 3.8 | 🔴 Below target |
| Post-escalation CSAT | 2.8 | — | ⚠️ Worse than bot-only |
| Avg chatbot session length (all sessions) | 8.4 min | — | — |
| Avg session length, resolved by bot | 4.2 min | — | — |
| Avg session length, escalated | 11.7 min | — | — |
| Session abandonment rate | 34% | — | ⚠️ No target set, high |
| Median time to first escalation response | 6.2 min | — | — |
| Avg time to full resolution (post-escalation) | 3.8 hrs | — | — |

---

## Funnel breakdown

```
7,240 sessions initiated
  │
  ├─ 2,454 (33.9%) — abandoned mid-session (no resolution, no escalation)
  │     ├─ Abandoned after bot couldn't understand: ~41%
  │     ├─ Abandoned after long back-and-forth: ~33%
  │     └─ Abandoned after receiving answer (likely resolved, unconfirmed): ~26%
  │
  ├─ 2,904 (40.1%) — resolved by bot (no human contact)
  │     ├─ Billing & payments: 31% of bot-resolved
  │     ├─ Account & permissions: 28% of bot-resolved
  │     ├─ Feature how-to: 24% of bot-resolved
  │     ├─ Technical configuration: 12% of bot-resolved
  │     └─ Technical bugs: 5% of bot-resolved
  │
  └─ 3,882 (53.6%) — escalated to human agent
        ├─ Bot-initiated escalation (bot gave up): 67%
        └─ User-initiated escalation (clicked "Talk to human"): 33%
```

Wait — note discrepancy: 33.9% abandoned + 40.1% resolved + 53.6% escalated = 127.6%. This is wrong. Let me re-run.

**Corrected funnel (re-run 2026-03-06):**

```
7,240 sessions initiated
  │
  ├─ 2,454 (33.9%) — abandoned mid-session
  ├─ 2,904 (40.1%) — resolved by bot
  └─ 1,882 (26.0%) — escalated to human agent
```

**Discrepancy note:** The original export double-counted sessions where users abandoned after the escalation request was triggered (bot initiated escalation, user left before agent joined). These sessions (~2,000) were being counted in both "abandoned" and "escalated." The escalation rate in other reports (59.9%) uses a broader definition — sessions where escalation was *requested*, regardless of whether the user was still present. True "completed escalation" rate is ~26%. This is an important definitional issue that affects how we read our KPIs.

*Recommend: align on a single escalation rate definition before Q2 planning. Currently we have two valid but different numbers.*

---

## Language segmentation

| Language of first user message | Sessions | % of total | Bot resolution rate | Escalation rate |
|---|---|---|---|---|
| English | 5,900 | 81.5% | 45.3% | 54.7% |
| Portuguese (BR) | 719 | 9.9% | 5.8% | 94.2% |
| Spanish | 421 | 5.8% | 6.4% | 93.6% |
| French | 102 | 1.4% | 29.4% | 70.6% |
| Other | 98 | 1.4% | 14.3% | 85.7% |

Portuguese and Spanish sessions have ~94% escalation rate vs. 55% for English. Bot is functionally non-functional for LATAM users.

1,140 LATAM sessions (PT + ES) in February alone. At ~$18/completed escalation, even if half of those complete the escalation flow, that's ~$10,000/month in cost directly attributable to language gap.

---

## Escalation quality signals

Of the ~1,882 completed escalations in February:

**Context handoff quality** (agent survey, n=312 sampled):
- "I received full conversation history from the bot": 8%
- "I received a partial summary": 41%
- "I received no context at all": 51%

**Actions taken by agent in first 2 minutes** (from ticket log analysis):
- Asked user to re-describe the problem: 74%
- Pulled up user account in separate system: 68%
- Looked up previous tickets: 31%
- Was able to proceed without re-collecting context: 12%

74% of agents ask the user to re-describe the problem — consistent with user reports (Sarah Chen: "80% of escalations"; Ryan Kim: "70-75%").

**Escalation → resolution time breakdown:**
- Median time: 3.4 hrs
- 90th percentile: 11.8 hrs
- Longest tail: cases that involve Tier 2 escalation (avg 18.6 hrs)

---

## Abandonment analysis

2,454 sessions abandoned mid-conversation. Exit point breakdown:

| Exit point in conversation | % of abandonments |
|---|---|
| After 1st bot response | 18% |
| After 2nd-3rd bot message | 26% |
| After back-and-forth (4-8 messages) | 31% |
| After bot acknowledged it can't help | 25% |

The "after bot acknowledged it can't help" group (25% of abandonments = ~614 sessions) is concerning — these are users who were explicitly told the bot couldn't solve their problem, but then didn't complete the escalation. They likely gave up entirely or went to another channel (email, direct ticket).

---

## Behavioral signals — pre-abandonment

For sessions that were abandoned, we looked at user behavior in the Acme app in the 10 minutes before they opened the chatbot:

| Prior action | % of abandoned sessions |
|---|---|
| Visited help center article | 44% |
| Encountered an error state / error message | 38% |
| Repeated same action 3+ times unsuccessfully | 29% |
| No notable prior behavior | 21% |

38% of users who eventually abandon the chatbot had hit an error state immediately before opening it. This is the clearest proactive trigger signal we have: users in error states are at high risk of needing help and failing to get it through the bot.

*Note: This data was not originally in scope for this report. Flagging to PM team as a potential signal for proactive engagement research.*

---

## Metric definitions used in this report

- **Deflection rate**: % of initiated sessions resolved by bot (no human agent contact, no ticket created)
- **Escalation rate (broad)**: % of sessions where escalation was requested (includes abandoned post-escalation-request)
- **Escalation rate (narrow/completed)**: % of sessions resulting in an agent actively engaging
- **Session CSAT**: Post-session survey (1-5 scale), sent after all resolved sessions; response rate ~33%
- **Abandonment**: Session where user disconnected without resolution and without escalation completing

---

## Open questions / data gaps

1. We can't distinguish "abandoned because resolved via other means" from "abandoned out of frustration." ~26% of abandonments may actually be silent resolutions.
2. Knowledge staleness attribution: we know 31% of escalations are due to incorrect/outdated answers (from ticket taxonomy), but we can't yet link specific bot responses to specific KB update dates in the funnel data.
3. Escalation definition discrepancy (see note above) needs to be resolved before we report Q1 KPIs to leadership.
