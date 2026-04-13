# Churn Postmortem Interview — TechCorp (Account Closed December 2025)
**Date:** 2026-01-10
**Interviewer:** James Wu (Senior CSM, Acme)
**Duration:** 28 minutes
**Format:** Phone call
**Interviewee:** David Park, Director of Engineering Operations, TechCorp
**Context:** TechCorp was an Acme client for 2 years (~90 seats, engineering org). They gave notice in November 2025 and migrated off Acme in December 2025. James conducted this exit interview to understand the root cause. TechCorp has since moved to a competitor (not disclosed to us but likely Notion + Zendesk based on their LinkedIn job postings).

---

## Pre-interview notes

TechCorp was a strong account — 2 years, high adoption, attended our user conference in 2024. Their churn was flagged as a surprise. The NPS they gave us in Q3 2025 was 7 (passive). The drop happened fast: NPS to churn in ~3 months.

Revenue impact: $68,000 ARR lost.

---

## Transcript (condensed, paraphrased except where quoted)

**On why they left:**
David was direct. He said it was "multiple things" but when pushed, he identified a specific sequence of events that built up over 3 months.

In September 2025, TechCorp had a critical incident: their Acme workspace was misconfigured after a migration of their team structure (they'd merged two departments). A bulk update of permissions permissions was applied incorrectly, which cascaded into ~30 engineers losing access to active project boards. This happened on a Friday afternoon.

They went to the chatbot. The bot gave them troubleshooting steps for the wrong version of the permissions system — steps that had been superseded in Acme's August 2025 update. Following those steps made the problem worse. The bot then escalated.

> "The agent who picked it up had no idea what we'd already tried. He actually suggested we do the thing we'd just told the bot made it worse. And then he said he'd need to escalate to Tier 2 and that could take 24-48 hours on a weekend."

TechCorp's engineers spent Friday night and Saturday morning manually reconstructing permissions one by one. No further bot use. They submitted a formal complaint.

**On the response to the complaint:**
They got a call from their CSM (not James at the time — previous CSM) who apologized and offered a service credit. David accepted it but said, "An apology doesn't give me back the weekend my team lost."

In October and November 2025, two more incidents occurred — both smaller, but both followed the same pattern: bot gave outdated guidance, escalation lost context, agent started from scratch. David said by November, his team had "informally banned" using the chatbot for anything beyond trivial questions.

> "We told the team: if it's real, open a ticket directly. Don't bother with the bot. And that's when I realized we were paying for something we'd stopped using."

**On the decision to leave:**
He framed it clearly: "We didn't leave because of the incidents. Incidents happen. We left because the pattern didn't change. After the September thing, we expected improvement. There was none."

David confirmed they evaluated 3 alternatives. Price was not the primary factor. The tool they chose had "a better support experience, specifically around how the chatbot hands off to humans — the agent actually sees everything."

**On the chatbot specifically:**
He said he understood chatbots are "immature technology" and had some patience for that in 2024. By 2025, he expected more. He felt the core failure was systemic: "The bot and the humans aren't connected. They're two separate things pretending to be one support experience."

He also flagged that the bot's knowledge about newer features was consistently behind: "Every time Acme shipped something new, the bot didn't know about it for weeks. Sometimes months."

**On what would have kept them:**
> "If after September, someone had said 'we're fixing the escalation flow — here's what that means, here's the timeline,' I think we'd have waited. Nobody said that. Nobody committed to anything."

---

## Interviewer observations

This is a pattern churn, not a moment churn. Three separate incidents, same failure mode each time. TechCorp didn't leave angry — they left resigned.

The "informal ban" on the chatbot is the most damning signal. If users bypass the product entirely, the product is providing zero value — and they're paying for it.

The competitor advantage named explicitly: agent sees the full conversation history. This is the same gap Sarah Chen identified.

The knowledge staleness issue appears in both this interview and Sarah Chen's. Likely systemic.

The "nobody committed to anything" quote is a relationship failure but enabled by the product failure — if we had a fix to commit to, CS could have committed.

---

## Raw quotes (verbatim)

- "The bot and the humans aren't connected. They're two separate things pretending to be one support experience."
- "We told the team: if it's real, open a ticket directly. Don't bother with the bot."
- "We left because the pattern didn't change."
- "If someone had said 'we're fixing the escalation flow — here's the timeline,' I think we'd have waited."
- "An apology doesn't give me back the weekend my team lost."

---

## Financial / business impact

- ARR lost: $68,000
- Competitor gained: unknown but likely ~$60-80k ARR equivalent
- Estimated incident cost to TechCorp: ~40 engineering hours (Friday night + Saturday) + executive time

---

## Follow-up actions

- [ ] Conduct internal post-mortem on September 2025 incident with support ops
- [ ] Review knowledge base update lag time — when do new features appear in bot KB?
- [ ] Flag escalation context-passing gap to PM (Priya) — this has now appeared in 2 separate interviews
- [ ] Review other accounts with 3+ escalations in 90-day window — churn risk cluster
