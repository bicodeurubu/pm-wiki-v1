# Internal Interview — Customer Support Team (Voice of the Agent)
**Date:** 2026-02-20
**Interviewer:** Priya Nair (Senior PM, Acme Support Products)
**Duration:** 55 minutes
**Format:** In-person, São Francisco HQ, Conference Room B
**Participants:**
- **Linda Torres** — Support Team Lead, 4 years at Acme
- **Ryan Kim** — Support Agent, L2, 2 years at Acme (handles complex escalations)
- **Aisha Patel** — Support Agent, L1, 8 months at Acme (most experience with current bot)
**Context:** Internal session to understand how the support team experiences the chatbot — what they see when they receive escalations, what's working, what's not. This is the "agent perspective" to complement the user research we've already done.

---

## Pre-interview notes

This was requested by Linda after she saw the TechCorp churn postmortem summary circulating internally. She reached out directly: "I have things to say about this." Treated as exploratory — no structured script, open discussion format.

Important framing: the CS team may have defensive instincts since chatbot success = fewer L1 tickets = potential headcount implications. Priya acknowledged this upfront and framed the session as "how do we make your job better," not "how do we replace you."

---

## Transcript (condensed, paraphrased except where quoted)

### Opening — Linda's framing

Linda opened with something unexpected: she said she thinks "the bot is actually decent, better than people give it credit for." She was pushing back on what she called "the narrative that the bot is broken."

Her position: the bot handles simple volume well. About 40% of chats never reach her team, and of those deflected cases, she rarely sees re-opens (users coming back after the bot). "The bot is doing its job for the easy stuff. Nobody talks about that."

This is a different framing than what users say. Worth noting.

### The escalation handoff — Ryan's perspective

Ryan handles escalated cases, so he has the most exposure to the quality problem. His view was blunter than Linda's:

> "When I get an escalation from the bot, I have no idea what the user already tried. I see a name, a company, and maybe a one-line summary that the bot auto-generates. Half the time that summary is wrong or totally vague — something like 'user has a technical issue.' That tells me nothing."

He described his workflow: when he gets an escalation, he opens the chat, checks if there's a summary (usually there isn't), and then asks the user to "quickly recap" what the problem is. He knows users hate this. "I can tell they're frustrated the moment I say it. They've been at this for 20 minutes already."

Ryan's observation on the auto-summary: "The bot generates a summary when it escalates, but it summarizes the last 2-3 messages, not the whole conversation. So if someone spent 15 minutes troubleshooting and then described their problem differently at the end, I only see the end."

He estimates 70-75% of his escalations require him to re-collect context. The ones that don't are usually very new users with simple first-time issues.

> "The worst ones are where the user says 'the bot told me to try X' and I can see they've done something wrong based on that advice, but I can't see what the bot actually said. I'm debugging blind."

### What Aisha sees as L1

Aisha's role is front-line — she sees the chats the bot can't resolve and the ones where users click "Talk to a human" proactively. She raised something neither Linda nor Ryan mentioned:

> "A lot of users hit 'talk to a human' not because the bot can't answer — but because they don't trust the bot's answer. They want a human to confirm."

She said this happens maybe 30-40% of the time in her queue. The bot gave an answer, user wasn't sure if it was right, asked for human confirmation. "So in those cases, the escalation is a confidence problem, not a capability problem."

She also flagged the multilingual issue unprompted — "we get tickets in Spanish and Portuguese. We don't have Spanish or Portuguese speakers on the team. Those go into a queue and wait for our offshore team, which can be 6-12 hours. The users have usually given up by then."

### What "good" looks like — Linda's ask

When asked what would most improve her team's experience:

**Linda:** "Give us the full conversation log in the escalation ticket. Not a summary — the whole thing. And flag what the bot already tried."

**Ryan:** "If I could also see the user's account context — what plan they're on, recent activity in Acme, if they've had similar issues before — I could solve things so much faster. Right now I have to pull that up manually in a separate tab."

**Aisha:** "For the confidence-problem escalations, maybe the bot could do a better job of saying 'I'm confident this is right' vs 'I'm not sure, here's my best answer.' If users knew the bot was uncertain, they'd escalate less and also trust it more when it is certain."

### The tension point — headcount

Toward the end, Linda raised the elephant in the room: "I want to be clear — I'm not trying to block the bot from getting better. I know what it means if deflection goes up. But I'd rather have 20 good cases a day than 60 bad ones." 

She's essentially endorsing quality over volume — which aligns with improving escalation handling, not just raw deflection rate.

### On knowledge staleness

Ryan brought this up independently (corroborates TechCorp and Sarah Chen interviews): "The bot is almost always wrong about new features for the first few weeks after a release. We launch something, users ask the bot, bot says something outdated, it escalates to us, and we're the ones who have to explain the new feature. We've become the human patch for bad bot knowledge."

He estimated this happens for every major feature release, with a lag of 2-6 weeks before the bot is updated. "That's a long time when users are trying to use a feature that just shipped."

---

## Key tensions / contradictions to flag

- Linda: "The bot is actually decent." vs. Sarah Chen and TechCorp: "The bot is the problem."
  - Resolution: Linda is talking about deflection quality (right cases get deflected). Users are talking about escalation quality (wrong cases get handed off badly). Both can be true simultaneously.
- Aisha: ~30-40% of escalations are "confidence problems" — users who got the right answer but don't trust it. If true, this means some escalation reduction could come from confidence UX, not capability improvements.
- Ryan: 70-75% of escalations require re-collecting context. Users estimated ~80% (Sarah Chen). Numbers align.

---

## Raw quotes (verbatim)

- "The bot is doing its job for the easy stuff. Nobody talks about that." (Linda)
- "I'm debugging blind." (Ryan)
- "The escalation is a confidence problem, not a capability problem." (Aisha)
- "We've become the human patch for bad bot knowledge." (Ryan)
- "I'd rather have 20 good cases a day than 60 bad ones." (Linda)

---

## Follow-up actions

- [ ] Investigate: what does the current escalation payload look like technically? What data is passed? (Eng)
- [ ] Quantify Aisha's "confidence escalation" hypothesis — can we tag escalations by reason in the data?
- [ ] Knowledge staleness lag: pull data on average time from feature release to bot KB update
- [ ] Consider: agent context panel as part of escalation v2 spec
