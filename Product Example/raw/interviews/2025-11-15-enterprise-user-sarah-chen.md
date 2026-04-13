# User Interview — Sarah Chen, VP Customer Success, RetailMax
**Date:** 2025-11-15
**Interviewer:** Priya Nair (Senior PM, Acme Support Products)
**Duration:** 42 minutes
**Format:** Video call (Zoom)
**Context:** RetailMax has been an Acme client for 3 years, ~140 seats, project management for their merchandising and supply chain ops. Support Chatbot rolled out to them in May 2025 as part of general availability.

---

## Pre-interview notes

RetailMax flagged an issue in their quarterly business review: their internal helpdesk team is complaining about the quality of escalations coming from the chatbot. We set this up to understand the friction better. Sarah manages a team of 6 internal "Acme power users" who handle project configuration and onboarding for new employees at RetailMax.

---

## Transcript (condensed, paraphrased except where quoted)

**On overall usage:**
Sarah said her team interacts with the chatbot "a few times a week" mostly for things like API questions, bulk import troubleshooting, and permission configuration. She acknowledged it's useful for simple, repetitive stuff — "if someone asks how to set up a recurring task, the bot nails it." The issue isn't the easy stuff.

**On escalations — the core problem:**
"The thing that drives my team absolutely crazy is when we have a real issue, something complex, and the bot tells us it's escalating to a human — and then the human starts from zero. I mean completely from zero. They ask us to describe the problem again, they have no idea what we already tried, they don't even know what the bot told us."

She gave a specific example from October 2025: her team had a critical issue with a bulk import of ~2,000 project templates that was corrupting metadata fields. The chatbot walked them through 3-4 troubleshooting steps (clearing cache, re-exporting in a specific format, checking field mappings) over about 25 minutes. When it escalated, the agent who took over had zero record of any of that. 

> "We spent another 20 minutes re-explaining everything we'd already done. And then the agent suggested the exact same steps the bot had already told us to try. That's when I started questioning whether the escalation is even worth it."

She mentioned her team has started doing a workaround: before escalating, they manually screenshot or copy-paste the bot conversation into the escalation chat. "We're doing the bot's job for it."

**On frequency:**
She estimates they hit a "real problem" (one that requires escalation) roughly 2-3 times per week. Of those, she said about 80% of escalations result in the agent needing to re-collect context. "Maybe once a month we get an agent who actually read the chat history. And I don't know if that's them or the system."

**On trust:**
"I've started to dread escalating. I'd rather spend an extra 30 minutes in the bot trying to solve it myself than go through that handoff. And that's not good either, because sometimes I'm wasting time when a human could fix it in 5 minutes if they had the context."

Interesting nuance: she doesn't blame the human agents. "The agents are fine. They're helpful when they understand the problem. The problem is the information doesn't travel with the ticket."

**On what she'd want:**
When asked what good looks like: "I want the agent to open the chat and say 'I see you've been troubleshooting X for 25 minutes, you've already tried Y and Z, let me pick up from here.' That's it. That's all I need."

She also mentioned wanting an option to flag severity before escalating: "Sometimes it's urgent — like a process is broken and 50 people are blocked. Sometimes it's not. The bot treats everything the same."

**On the bot's knowledge:**
She thinks the knowledge base is "decent for stable features, but always behind on new ones." Mentioned that the bulk import troubleshooting guidance in the bot was outdated — it was referencing a workflow that Acme had changed 2 versions ago.

**On willingness to stay:**
Not explicitly asked, but she mentioned they're "evaluating tools" in Q1 2026. No direct threat, but this is worth flagging.

---

## Interviewer observations

Sarah is articulate and not trying to churn — she's trying to get this fixed. The escalation handoff is the primary pain point, not the bot's coverage. The severity signaling idea is interesting and came up organically. The knowledge staleness is a secondary but real issue.

The workaround (manual copy-paste before escalating) is a strong signal — users building their own duct-tape solution = unmet need.

---

## Raw quotes (verbatim, from notes)

- "The information doesn't travel with the ticket."
- "I've started to dread escalating."
- "I'd rather spend 30 more minutes in the bot than go through that handoff."
- "We're doing the bot's job for it." (re: manual copy-paste workaround)
- "Maybe once a month we get an agent who actually read the chat history."

---

## Follow-up actions

- [ ] Check with eng: does the current escalation flow pass any conversation history to the agent? (Priya)
- [ ] Pull escalation data for RetailMax account specifically (data team)
- [ ] Schedule follow-up with RetailMax in Q1 2026 QBR
