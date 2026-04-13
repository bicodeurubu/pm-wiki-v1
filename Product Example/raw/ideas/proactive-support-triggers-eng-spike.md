# Spike Notes — Proactive Support Triggers (Eng Exploration)
**Author:** Elena Rodriguez (Senior Software Engineer, Platform Team)
**Date:** 2026-02-14
**Type:** Engineering spike / concept note — NOT a formal spec
**Audience:** PM team + tech leads (Priya, Marco)
**Status:** Rough — shared as "thinking out loud," needs PM validation before going further

---

## Background / what prompted this

During the February sprint retrospective, I raised that we're seeing a pattern in our error logs that bothers me: users hit an unhandled error state in the app, then within 30-60 seconds they open the chatbot, then the chatbot can't help them (it doesn't know what the error was), and then they escalate. We're seeing this sequence probably hundreds of times a week.

My question: what if the chatbot knew about the error before the user had to explain it?

I spent 3 days exploring this as an informal spike. Notes below.

---

## The core idea

**Proactive Support Triggers:** Instead of waiting for a user to open the chatbot and explain their problem, we detect signals in the product that indicate a user is struggling — and either:
1. Pre-load context into the chatbot so it's ready when the user opens it, or
2. Proactively open a chat (or surface a contextual help snippet) before the user even clicks "Help"

The hypothesis is that if we know what a user was doing when they got stuck, the chatbot can jump straight to the right answer instead of spending 5-10 messages figuring out what the problem is.

---

## Signals we could use (what we already have)

After reviewing our analytics pipeline and event tracking, here's what we're already logging that could trigger proactive support:

| Signal | Where it comes from | Quality for support triggering |
|---|---|---|
| Client-side error events (4xx/5xx from API calls) | Amplitude + server logs | High — specific, actionable |
| UI error state rendering (error toast, error page) | Amplitude (frontend events) | High — user-visible problem |
| Repeated same action within 60 seconds (retry behavior) | Amplitude | Medium — could be normal workflow |
| Session on help center article > 60 seconds | Amplitude | Medium — indicates confusion |
| Failed form submission (validation errors) | Amplitude | High for onboarding flows |
| Idle on "loading" state > 30 seconds | Amplitude | Medium — could be slow network |
| Chatbot opened within 2 min of error event | Amplitude (correlation) | Very high — confirmed struggle signal |

The last row is the most interesting: we can already see in our data that ~38% of chatbot sessions are opened within 2 minutes of a tracked error event. These users are coming to the chatbot *because* of the error. But the chatbot doesn't know this.

---

## What we'd need to build

### Option A — Context pre-loading (lower lift)
When a user opens the chatbot, check the last 5 minutes of their event stream. If there's an error event, pre-populate the chatbot context with:
- What the user was doing (e.g., "you were editing a bulk import job when this started")
- What error occurred (e.g., "we saw a 422 error on the /bulk-imports endpoint")
- A suggested first question or article

The user still opens the chat themselves. We just know what to say when they do.

**Estimated effort:** 2-3 sprints
- Amplitude → chatbot context bridge: ~1 sprint (new service or middleware)
- Chatbot UI changes (contextual welcome message): ~0.5 sprint
- LLM prompt updates (incorporate event context): ~0.5 sprint
- Testing + QA: ~1 sprint

**Risk:** Privacy/data sensitivity — are users comfortable with the chatbot "knowing" what they just did? Need to consider disclosure and opt-out.

### Option B — Proactive outreach (higher lift)
Chatbot proactively opens or sends a notification when a trigger fires, before the user clicks Help.

**Estimated effort:** 4-6 sprints
- All of Option A, plus:
- Real-time event stream processing (currently batch, not real-time)
- User notification/chat surface that opens proactively
- Rules engine for when to trigger (avoid spamming)
- A/B testing infrastructure for trigger thresholds

**Risk:** Much higher risk of being annoying/intrusive. Need very clear triggering logic and easy dismissal.

---

## Hypothesis (what we'd need to prove)

**Primary hypothesis:** Users who receive proactive context pre-loading will resolve their issue without escalation at a higher rate than users who don't, for the same error types.

**Success metric:** Deflection rate for error-triggered sessions goes from the current baseline (~18% for technical issues) to >40%.

**Secondary hypothesis:** Users who receive proactive support will open fewer follow-up tickets within 24 hrs of the initial interaction.

---

## Open questions for PM

1. **Is this the right priority?** I'm seeing it as a differentiator (Intercom and Zendesk do page-based proactive, not error-based). But I don't know if it's more or less important than multilingual support or escalation context handoff. That's a PM call.

2. **Who owns the event taxonomy?** If we want to pre-populate context, we need a maintained map of "error event → what it means in plain language → recommended first support action." That's content work, not just engineering. Who does that?

3. **Privacy posture:** Do we surface that we know about the user's error? "Hi, I see you ran into an issue with bulk imports — want help?" vs. "Hi, how can I help?" I lean toward transparent but want PM/legal input.

4. **Pilot approach:** Could we start with Option A for just one error type (e.g., bulk import failures, which are common and well-understood) before building the full system?

---

## Relevant data point I found while looking at this

(Sharing because it's relevant to other work, even if we don't build proactive triggers.)

In the February funnel data (Lena's report), 34% of chatbot sessions are abandoned mid-conversation. Of those, 38% had hit an error state in the app in the prior 10 minutes. That's roughly 320 sessions/month where a user was struggling with a product error, opened the chatbot, didn't get help, and left. Those 320 users likely submitted a ticket anyway or gave up.

That number will grow as we scale. Just thought it was worth flagging.

---

## What I'm not suggesting

This is not a proposal to replace human agents or aggressively deflect complex issues. I'm specifically thinking about the case where we know exactly what happened (an error we can identify) and could help immediately without making the user explain it.

I'm also not suggesting we do this before fixing escalation context handoff — that seems like a more urgent problem based on what I've heard from the PM team and the churn data.

---

## Next steps (if we want to explore)

- PM validates priority vs. other roadmap items
- 30-min sync between Priya, me, and Lena to align on event taxonomy and data availability
- If go: write a proper tech spec and get arch review before any implementation
