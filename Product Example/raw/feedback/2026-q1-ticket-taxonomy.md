# Q1 2026 Support Ticket Taxonomy & Pattern Analysis
**Period:** January 1 – March 31, 2026 (preliminary — data through March 15 extrapolated)
**Analyst:** Tomás Ferreira (CS Ops) + data pull by Lena Park (Data Engineer)
**Report date:** 2026-03-20
**Source:** Zendesk ticket export + chatbot conversation logs (Acme internal system)

---

## Total volume

| Channel | Tickets / chats | vs. Q4 2025 | vs. Q1 2025 |
|---|---|---|---|
| Chatbot sessions (initiated) | 21,340 | +12% | +89% |
| Chatbot resolved (no escalation) | 8,536 (40%) | +1pp | +4pp |
| Escalated to human | 12,804 (60%) | -1pp | -4pp |
| Direct ticket submissions (bypassing bot) | 3,218 | +28% | — (no baseline) |
| Total human-handled | 16,022 | +18% | — |

**Note on direct ticket submissions:** The 28% increase in direct submissions is significant. Anecdotally from CS team, some users have learned to bypass the bot entirely. This number was not tracked before Q4 2025 — we added the channel distinction after the TechCorp churn interview flagged the "informal ban" pattern.

---

## Ticket category breakdown (all human-handled tickets, n=16,022)

| Category | Volume | % of total | Bot deflection rate | Avg resolution time (human) |
|---|---|---|---|---|
| Technical — product bugs | 3,842 | 24% | 18% | 4.2 hrs |
| Technical — configuration | 3,204 | 20% | 31% | 2.8 hrs |
| Billing & payments | 3,524 | 22% | 68% | 0.9 hrs |
| Account & permissions | 2,884 | 18% | 64% | 1.1 hrs |
| Feature how-to / guidance | 1,602 | 10% | 71% | 0.7 hrs |
| Other / uncategorized | 966 | 6% | 22% | 3.1 hrs |

**Key observation:** Technical tickets (bugs + configuration = 44% of volume) have dramatically lower bot deflection rates (18-31%). This is where the bot is failing. Billing and account tickets (40% of volume) deflect well (64-68%).

The bot is essentially a strong billing/account chatbot and a weak technical support chatbot — but it's marketed as both.

---

## Escalation reason codes (n=12,804 bot-to-human escalations)

Starting Q1 2026, we added mandatory escalation tagging. L1 agents tag the primary reason for each escalation at intake.

| Escalation reason | Count | % |
|---|---|---|
| Bot answer was incorrect or outdated | 3,969 | 31% |
| Bot could not understand / misclassified intent | 2,689 | 21% |
| User requested human proactively (no bot failure) | 2,561 | 20% |
| Bot gave correct answer but user wanted confirmation | 1,921 | 15% |
| Language barrier — user non-English | 1,408 | 11% |
| Bot loop / technical failure | 256 | 2% |

**Observations:**
- "Incorrect or outdated answer" (31%) = knowledge staleness problem. Corroborates TechCorp interview and CS team interview.
- "User requested human proactively" (20%) + "wanted confirmation" (15%) = 35% of escalations are not bot failures — they're confidence/preference escalations. This matches Aisha's estimate from the internal interview.
- Language barrier escalations (11%) represent 1,408 escalations in Q1. At ~$18/escalation cost (blended), that's ~$25,000 in Q1 alone attributable to language gaps.

---

## Language distribution in escalations

Of the 1,408 language-barrier escalations:

| Language | Count | % of language escalations |
|---|---|---|
| Portuguese (Brazil) | 718 | 51% |
| Spanish (Colombia/Mexico) | 512 | 36% |
| French | 98 | 7% |
| Other | 80 | 6% |

Portuguese and Spanish together = 87% of language-barrier escalations.

**Separate signal:** Of all chatbot sessions (n=21,340), we can infer language from the initial user message. Approximately 18-19% of sessions begin with a non-English first message. Of those, the escalation rate is 92% (vs. 60% overall). Non-English users almost always escalate.

---

## Knowledge staleness — feature-specific breakdown

The "incorrect or outdated answer" escalations (n=3,969) were manually sampled (n=200) to identify which feature areas are most affected.

| Feature area | % of sampled "outdated" escalations | Last bot KB update |
|---|---|---|
| Route optimization (new in Nov 2025) | 34% | 2026-02-01 (2+ months lag) |
| Bulk import / template management | 28% | 2025-11-15 (but feature changed Dec 2025) |
| API v3 / integrations | 19% | 2025-09-30 (very stale) |
| Permissions & roles | 11% | 2026-01-10 |
| Other | 8% | — |

The route optimization feature shipped November 2025. The bot KB wasn't updated until February 2026 — a 2+ month lag. During that period, every user question about route optimization was answered incorrectly or met with "I don't have information on that."

---

## CSAT by support path

| Path | CSAT (avg, 1-5 scale) | n |
|---|---|---|
| Bot resolved (no escalation) | 3.4 | 2,841 (33% response rate) |
| Bot → escalation → resolved | 2.7 | 1,890 (15% response rate) |
| Direct ticket → resolved | 3.6 | 890 (28% response rate) |

**Critical insight:** Post-escalation CSAT (2.7) is *lower* than bot-only CSAT (3.4) AND lower than direct ticket CSAT (3.6). The act of escalating through the bot produces worse satisfaction than either alternative. This suggests escalation is actively damaging the experience — not saving it.

Direct ticket CSAT being highest is also a flag: users who bypass the bot entirely report better experiences.

---

## Repeat contact rate

Users who submitted more than one support request within 30 days of a prior interaction:

- After bot resolution: 14% repeat contact rate
- After bot → escalation: 38% repeat contact rate
- Industry benchmark (Gartner 2025): 22% acceptable repeat contact rate

Post-escalation repeat contact rate (38%) is nearly double the benchmark. Escalations are not fully resolving issues.

---

## Recommendations from this analysis

1. **Address knowledge staleness as a process problem**, not a one-off. The 2+ month lag on route optimization is not exceptional — it's the pattern.
2. **Redesign escalation for the 35% "confidence/preference" segment**: these are solvable without human agents if the bot communicated confidence levels.
3. **Prioritize PT + ES language support**: 87% of language escalations, $25k+ quarterly cost, concentrated in LATAM expansion accounts.
4. **Track direct bypass separately and take it seriously**: 3,218 direct tickets in Q1 = users who have given up on the bot. This number should be going down, not up.
