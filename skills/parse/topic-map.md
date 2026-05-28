# Topic → Goal map

Per-goal matching data used by `parse` to classify user messages. For classification rules (Strong / Medium / None) and decision logic, see `SKILL.md`.

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

**Nudge:** *If you'd like, I can run a quick analysis on the sample data to make this concrete — just ask.*

---

## learn-about-treasure-ai

**What it covers:** questions about Treasure AI, the platform, its features, capabilities, pricing, how it works, what it does, comparisons with other CDPs or marketing tools.

**A message fits this goal if** the user is asking about Treasure AI itself — what it is, what it can do, how it compares, pricing, or any product-level question.

**Example phrasings:**
- "What is Treasure AI?" / "What does this do?" / "What can you do?"
- "How does this work?" / "How is this different from Segment?"
- "Is this free?" / "How does pricing work?" / "What plan do I need?"
- "What's a CDP?" / "What is AI Studio?"
- "What features do you have?" / "Can you do X?"

**Strong-intent signal:** none — this goal is always Medium or dispatched from None.

**Nudge:** *If you'd like, I can show you what Treasure AI can do with a hands-on demo — just pick a topic.*

---

## general-questions

**What it covers:** anything that doesn't fit any other goal — truly off-topic, non-marketing, meta, casual, or general knowledge questions.

**A message fits this goal if** the user is asking something unrelated to marketing, data, or Treasure AI. This is the catch-all.

**Example phrasings:**
- "What's the weather?" / "Tell me a joke"
- "Who built this?" / "What model are you running on?"
- "Hi" / "Hello" / "What time is it?"

**Strong-intent signal:** none — this goal handles casual and off-topic messages.

**Nudge:** *By the way, if you're curious about what Treasure AI can do with your customer data — just ask!*

