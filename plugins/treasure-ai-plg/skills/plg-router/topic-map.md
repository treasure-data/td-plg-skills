# Topic → Goal map

Used by `plg-router` to decide which goal a user's message is about.

## How to match

For each goal below, read its **semantic description** and ask: *"Does the user's message fit this description?"* Use your judgment — the example phrasings are anchors, not a closed list. Synonyms, paraphrases, industry-specific terms, and metaphors all count if the meaning matches.

A user message can match:
- **One goal** → Medium intent on that goal (or Strong, if they're asking for the artifact).
- **Multiple goals** → Medium intent. Pick the most specific match, or ask the user which they meant.
- **No goal** → None intent. Use the generic fallback nudge.

When in doubt between None and Medium, **default to Medium** — better to over-offer a playbook than leave the user with no path forward.

---

## journey-planning

**What it covers:** multi-step automated customer experiences — lifecycle flows, sequences triggered by behavior, re-engaging lapsed customers, welcome series, win-back, retention sequences, loyalty flows, automated touchpoints over time.

**A message fits this goal if** the user is describing an automated sequence of interactions over time, asking how to bring a group of customers back, or asking how to move customers through stages.

**Example phrasings:**
- "lapsed customers" / "dormant users" / "customers who stopped buying" / "ghost customers"
- "win them back" / "re-engagement" / "revive my list" / "wake up inactive users"
- "what happens after signup" / "onboarding flow" / "drip" / "nurture sequence"
- "journey" / "lifecycle" / "automation" / "triggered messages"
- "welcome series" / "loyalty program" / "retention flow"
- comparisons: "journey vs workflow" / "journey vs campaign" / "journey vs segment"

**Strong-intent signal:** asking to *build / design / sketch / map / plan / create* a specific journey for a specific audience.

**Nudge:** *If you'd like, I can sketch a sample journey for this scenario — just say the word.*

---

## campaign-planning

**What it covers:** single email sends — subject lines, copy, send timing, A/B tests, promotional blasts, newsletters, announcements, the metrics that measure them at the message level (open rate, click-through, etc.).

**A message fits this goal if** the user is asking about composing, sending, or measuring a specific email or batch of emails — the message itself, not the broader sequence.

**Example phrasings:**
- "subject line" / "email copy" / "creative" / "preview text"
- "open rate" / "click-through" / "CTR" / "send time" / "best day to send"
- "A/B test" / "promotional email" / "newsletter" / "blast" / "announcement"
- "email campaign" / "marketing email" / "promo email"
- "what makes a good email" / "how do I write" + email context

**Strong-intent signal:** asking to *draft / write / plan / design* a campaign for a specific goal/audience.

**Distinguishing from journey-planning:** if the focus is one message → campaign. If the focus is the *sequence* of messages → journey.
**Distinguishing from generate-report:** "How is my campaign performing?" → generate-report (measurement), not campaign-planning (creation).

**Nudge:** *If you'd like, I can help you plan a sample email campaign with realistic targets — just ask.*

---

## find-segment

**What it covers:** defining audiences — who fits which group, what criteria define a cohort, attribute and behavior filters, customer profiles, who-bought-what queries used to scope a group rather than analyze one.

**A message fits this goal if** the user is asking how to identify, define, or describe a group of customers based on attributes or behaviors — the *who*, not the *what to do with them*.

**Example phrasings:**
- "segment" / "audience" / "cohort" / "customer group"
- "VIP" / "high-value" / "best customers" / "top customers" / "whales"
- "criteria" / "filter customers" / "find customers who..." / "who bought" / "who didn't buy"
- "target customers for X" (when the focus is identifying, not activating)
- comparisons: "segment vs audience" / "segment vs journey"

**Strong-intent signal:** asking to *find / build / define / list / show* a specific group with concrete criteria.

**Distinguishing from journey-planning:** segment = the audience definition; journey = what happens to that audience over time. If the user wants both, treat as Medium and ask which they want first.

**Nudge:** *If you'd like to see this in action, I can build a sample customer segment with you — just say the word.*

---

## analyze-data

**What it covers:** ad-hoc data exploration — "what does the data say," distributions, raw-data trends, profiling, looking for patterns or correlations in the data itself.

**A message fits this goal if** the user is asking to look at data and surface what's interesting, without specifying a fixed metric or report format.

**Example phrasings:**
- "analyze" / "explore data" / "look at the data" / "dig into"
- "patterns" / "trends" (when not paired with KPIs/reports) / "correlation" / "distribution"
- "what's in my data" / "what does the data show" / "data tells us"
- "basket analysis" / "cohort analysis" / "purchase patterns" / "seasonality"

**Strong-intent signal:** asking to *show / chart / analyze / explore* a specific dataset or relationship.

**Distinguishing from generate-report:** analyze = open-ended exploration. Report = fixed structure with KPIs and time period.

**Nudge:** *If you'd like, I can run a quick analysis on the sample data to make this concrete — just ask.*

---

## generate-report

**What it covers:** structured reporting — KPIs, dashboards, executive summaries, performance reviews, periodic reporting (weekly/monthly/quarterly), the difference between metrics and KPIs, how to present results to stakeholders.

**A message fits this goal if** the user is asking about measuring performance against defined metrics, packaging insights for an audience, or anything involving the words KPI / dashboard / report / scorecard.

**Example phrasings:**
- "report" / "dashboard" / "scorecard" / "executive summary"
- "KPI" / "metric" / "metrics overview" / "what should I track"
- "weekly / monthly / quarterly review" / "QBR"
- "how is X performing" / "how are we doing" / "performance review"
- comparisons: "KPI vs metric" / "dashboard vs report"

**Strong-intent signal:** asking to *generate / build / put together / compile* a specific report or dashboard for a period.

**Nudge:** *If you'd like, I can put together a sample performance report on the demo data — just say the word.*

---

## explore

**What it covers:** the user wants to do something hands-on but isn't sure what, or wants a guided tour. This is also the "Explore freely" choice from onboarding.

**A message fits this goal if** the user is gesturing at "show me what's possible" without committing to a specific outcome.

**Example phrasings:**
- "play around" / "try it out" / "show me what you can do"
- "tour" / "playground" / "show me around" / "what can this do"

**Nudge:** *If you'd like, I can give you a quick guided tour of what's possible here — just say "show me around".*

---

## None-intent fallback

When the user's message **doesn't fit any goal description above** — it's truly off-topic, meta, or generic — use this fallback nudge after answering:

> *If you'd like, I can walk you through a quick setup to tailor things to your industry and goals — just say `/onboarding` or "walk me through setup".*

Examples that should use the generic fallback:
- "Who built this?"
- "Is this free?" / "How does pricing work?"
- "What's the weather?" / "Tell me a joke"
- "What model are you running on?"

---

## Disambiguation guide

When a message could fit multiple goals, use these rules:

- **One message vs. a sequence of messages** → campaign (one) vs. journey (sequence).
- **Defining a group vs. acting on a group** → find-segment (defining) vs. journey-planning / campaign-planning (acting).
- **Open-ended exploration vs. structured measurement** → analyze-data (open) vs. generate-report (structured KPIs).
- **Creating vs. measuring** → campaign-planning / journey-planning (create) vs. generate-report (measure).
- If still ambiguous, treat as Medium and ask the user which direction they want.

## "What is X?" rule

Any "What is X?" / "What's a X?" / "Define X" / "What does X mean?" question, where X is a concept that fits one of the goal descriptions above, is **always Medium intent** on that goal — even if X isn't in the example phrasings list. Use your judgment about whether X belongs to a goal's domain.

## Comparison rule

Any "What's the difference between X and Y?" / "X vs Y" / "How does X compare to Y?" question, where X or Y fits a goal description, is **always Medium intent** on that goal. If X and Y span two different goals, pick the more specific one or ask the user.
