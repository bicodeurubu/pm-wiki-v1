# Q4 2025 NPS Survey — Support Experience Analysis
**Survey period:** October 1 – December 15, 2025
**Distributed by:** Customer Success Operations
**Analyst:** Tomás Ferreira (CS Ops)
**Report date:** 2026-01-08

---

## Survey overview

This is the quarterly NPS survey focused specifically on support experience (not overall product NPS). Sent to primary contacts at all active accounts with at least 30 days of chatbot exposure.

- **Sent:** 612
- **Responded:** 234 (38.2% response rate — up from 31% last quarter)
- **Accounts represented:** 198 unique accounts

---

## NPS Score

**Q4 2025 NPS: 32**

Historical comparison:
| Quarter | NPS | Notes |
|---|---|---|
| Q4 2024 | 41 | Pre-chatbot launch |
| Q1 2025 | 39 | Chatbot in beta (limited rollout) |
| Q2 2025 | 36 | Chatbot GA launch (May 2025) |
| Q3 2025 | 34 | First full quarter post-GA |
| Q4 2025 | 32 | ← Current |

NPS has declined 9 points since the chatbot launched. However, note that overall ticket volume has also increased significantly in this period due to new feature launches — so causal attribution is unclear. See data gaps section.

**Breakdown:**
- Promoters (9-10): 29%
- Passives (7-8): 34%
- Detractors (0-6): 37%

---

## Top pain points (open-text analysis, n=234)

Respondents were asked: *"What is the single biggest frustration with Acme's support experience?"*

Responses were coded into themes. Multi-code allowed where responses mentioned multiple issues.

| Theme | % of responses | Representative quote |
|---|---|---|
| Slow resolution / escalation delays | 43% | "When I need a real human, the wait is too long and they don't know what the bot already said." |
| Bot doesn't understand my problem | 38% | "I type exactly what I need and it gives me answers to a different question." |
| Language barrier | 31% | "Everything is in English. My team struggles." |
| Knowledge feels outdated | 27% | "The bot tells me to do things that don't match what I see in the product." |
| Had to repeat myself to human agent | 22% | "Every escalation feels like starting over." |
| Hard to reach a human when I need one | 19% | "Sometimes I just want to talk to a person. The bot makes that hard." |

**Observation:** "Had to repeat myself" (22%) is coded separately from "slow resolution" (43%) but they likely co-occur. When we look at co-occurrence, 81% of respondents who flagged slow resolution also flagged some form of context loss.

---

## Verbatim highlights

**Detractors:**

> "Your chatbot is a wall between me and someone who can actually help. It delays everything and adds nothing."
> — Detractor, 250-seat enterprise account, financial services

> "We gave up using the chatbot months ago. Now we email directly. Faster that way."
> — Detractor, 60-seat account (note: this is the "bot bypass" behavior — consistent with CS team interview)

> "The bot answered my question in English. We are based in Colombia. This is a problem."
> — Detractor, 45-seat account, Colombia (LATAM cohort)

> "Every time there's a new feature, the chatbot acts like it doesn't exist for weeks."
> — Detractor, 110-seat account, tech industry

**Passives:**

> "It's okay for simple stuff. When it gets complicated, it falls apart."
> — Passive, 80-seat account

> "I've lowered my expectations. When the bot actually solves something, I'm pleasantly surprised."
> — Passive, 200-seat account (this should worry us — managed expectations ≠ satisfaction)

**Promoters:**

> "When I have a routine question, the bot is fast and accurate. That part works really well."
> — Promoter, 55-seat account

> "The billing help through the chat is excellent. I never need a human for that."
> — Promoter, 90-seat account (corroborates ticket data: billing = high deflection quality)

---

## Segment analysis

**By account size:**
- Enterprise (200+ seats): NPS 18 (significant drop from 34 in Q3)
- Mid-market (50-199 seats): NPS 35
- SMB (<50 seats): NPS 44

Enterprise NPS drop is notable — large accounts have more complex support needs and suffer disproportionately from escalation quality issues.

**By region:**
- North America: NPS 38
- LATAM: NPS 11 (n=31, small sample but stark)
- Europe: NPS 29

LATAM NPS of 11 is alarming. These are exactly the accounts where language is a barrier. Small sample — should be treated as directional, not definitive.

**By industry:**
- Tech/SaaS: NPS 27
- Retail/E-commerce: NPS 34
- Logistics: NPS 29
- Financial Services: NPS 22

Tech/SaaS detractors tend to have more technical support needs — aligns with ticket taxonomy data showing technical tickets have lowest bot deflection rate.

---

## What promoters have in common

Promoters cluster around accounts that primarily use the bot for billing and account management questions — the highest-deflection, highest-accuracy categories. They also tend to be smaller accounts with simpler project configurations.

This suggests the bot is doing well in a specific, constrained use case (routine transactional support) and struggling in complex technical and multilingual scenarios.

---

## Data gaps and caveats

- Response rate is 38% — non-response bias is possible. Accounts experiencing the most frustration may have lower motivation to complete surveys.
- NPS decline correlates with chatbot GA launch but also with increased product complexity (we shipped 3 major features in H2 2025). Cannot cleanly attribute NPS change to chatbot alone.
- LATAM sample (n=31) is too small for statistical significance. Treat as directional.
- The survey doesn't distinguish between "bot escalation" and "direct ticket submission" as support paths — so "slow resolution" sentiment may be mixing experiences.

---

## Recommended next steps

1. Follow-up qual research with enterprise detractors (already partially done — TechCorp, Sarah Chen)
2. Dedicated LATAM survey or interviews (n=31 is insufficient)
3. Separate bot-path vs. direct-ticket path in next survey instrument
