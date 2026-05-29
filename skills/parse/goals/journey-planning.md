# Goal: Journey Planning

Plan a CDP customer journey in 5 steps: setup via FormCard, generate a skeleton YAML, build out stages, refine outcomes and pacing, then finalize the journey plan brief. This is PLANNING ONLY — produce a journey plan document. Do NOT activate, push, or deploy the journey.

**This flow is for demo purposes only.** We will NOT make any changes to the demo data or environment. No segments, activations, or journeys will be created, modified, or deleted. The output is purely a plan/preview document.

## Workflow

1. **Greet the user** — write a brief, natural greeting that reflects what the user actually asked for. Mirror their language and energy. Be concise and action-oriented. Do not use a canned phrase — tailor the greeting to their specific request.

2. **Ask journey type, industry & data source** — before the form, briefly explain what journey orchestration involves (1–2 sentences).

   If journey type, industry, and/or data source were already provided in the user's initial prompt (or a prior step), **omit those questions** — use those values directly and only ask the remaining questions. If all three are already known, skip this step entirely and proceed to Step 1 (Journey Setup). Otherwise, include only the missing questions in a single AskUserQuestion with `layout: 'form'`:

   **Question 1 — Journey type** (single-select, skip if already known):
   - Header: "Type"
   - Question: "What type of customer journey would you like to plan?"
   - Options:

     | Label | Description |
     |-------|-------------|
     | Welcome / onboarding series | Guide new customers through their first experience |
     | Re-engagement / win-back | Bring back inactive or lapsed customers |
     | Upsell / cross-sell | Drive additional purchases from existing customers |
     | Loyalty program | Reward and retain your best customers |

   **Question 2 — Industry** (single-select, skip if already known):
   - Header: "Industry"
   - Question: "What industry are you in?"
   - Options:

     | Label | Description |
     |-------|-------------|
     | Retail | |
     | CPG | |
     | Travel | |
     | Automotive | |
     | Media | |
     | D2C | |
     | B2B Tech | |

   **Question 3 — Data source** (single-select, skip if already known):
   - Header: "Data"
   - Question: "Which data source should we work with?"
   - Options:

     | Label | Description |
     |-------|-------------|
     | Synthetic data | Use pre-loaded sample datasets to explore features |
     | Upload my own data | Import CSV, JSON, or other files |

   After the user answers:
   - Tailor terminology, examples, and KPIs to their industry for the rest of the session.
   - If synthetic data: use pre-loaded sample datasets to explore features.
   - If upload: remind the user to upload via the "+" icon in the chat window and wait for the file attachment before proceeding. If data is already uploaded, use that. Avoid personal or confidential data.

3. **Five steps**: Setup (FormCard) → Skeleton YAML → Build out stages → Refine outcomes → Summary artifact.

### Step 1: Journey Setup

Briefly explain what a customer journey is and what we need to configure (1–2 sentences): *"A customer journey is an automated sequence of messages and actions triggered by customer behavior. We'll define who enters it and which channels to use."*

Use a single AskUserQuestion with `layout: 'form'`:

**Question 1 — Target audience** (free-text):
- Header: "Audience"
- Question: "Who should enter this journey? Describe the target audience."
- options: [] (empty — renders as text input)
- allowOther: true
- defaultOtherText: "New signups in the last 7 days"

**Question 2 — Channels** (multi-select):
- Header: "Channels"
- Question: "Which channels should this journey use?"
- defaultValue: ["Email"]
- Options:

  | Label | Description |
  |-------|-------------|
  | Email | Send email campaigns |
  | SMS | Send text messages |
  | Push notification | Send mobile push notifications |
  | In-app message | Show messages inside the app |
  | Paid media | Target via advertising platforms |

After submission, if "Other" is selected, ask one free-text follow-up for the scenario description.

### Generate Initial Journey Plan Brief

After collecting all Step 1 inputs, generate an initial journey plan as a markdown file named `[journey-name]-plan.md`. The journey name should be auto-generated from the type + industry + audience (e.g., `retail-welcome-new-signups-plan.md`).

The initial `.md` file should contain:

```markdown
# [Journey Name] — Journey Plan

## Next Steps
- [ ] Generate journey structure
- [ ] Build out stage details
- [ ] Define success metrics and pacing
- [ ] Finalize journey plan

## Overview
- **Journey Type:** [selected journey type]
- **Industry:** [selected industry]
- **Data Source:** [selected data source]
- **Target Audience:** [audience description from free-text input]
- **Channels:** [comma-separated list of selected channels]

## Industry Context
[1–2 sentences on industry-specific journey orchestration context]

## Journey Strategy
[2–3 sentences outlining the strategic direction based on journey type + industry + audience]

---
*Draft generated during journey planning. This document will be updated as we design the journey stages and refine the plan.*
```

Write this file to the working directory and call `mcp__tas__open_file` to display it. Tell the user: "I've created an initial journey plan. We'll update it as we design the stages and refine the journey."

Then proceed to Step 2.

### Step 2: Journey Skeleton YAML

Generate a `.journey.yaml` file with the high-level structure only. Start simple — 2 stages:
- Journey name (auto-generated from type + industry + audience)
- Description, goal segment definition
- 2 stage names with entry criteria only (no steps yet). Step 3 can expand to more stages if the journey type warrants it.
- Use the channels selected in Step 1

Call `mcp__tas__open_file` to display — the frontend automatically renders a visual journey diagram showing the stage structure. Let the user know: "I've created an initial journey diagram — this is a live document that will update as we build out the stages and as you provide feedback."

**Update the journey plan brief:** Add a `## Journey Structure` section to the `[journey-name]-plan.md` file:

```markdown
## Journey Structure
- **Journey name:** [auto-generated name from YAML]
- **Number of stages:** [count]
- **Stage overview:**
  1. **[Stage 1 Name]** — [entry criteria summary]
  2. **[Stage 2 Name]** — [entry criteria summary]

- **Goal segment:** [goal segment definition from YAML]
```

Check off "Generate journey structure" in Next Steps. Update the file and call `mcp__tas__open_file` to refresh it.

Then immediately proceed to Step 3 (no confirmation needed).

### Step 3: Build Out Stages

For each stage, add full detail to the existing `.journey.yaml`:
- Steps (activation, wait, decision_point, merge, end)
- Branch segment definitions (inline where used in the flow)
- Activation definitions with connector configs (inline where used in the flow)
- Milestone segment (what advances to next stage)
- Exit criteria

Present the plan stage by stage so the user sees one coherent plan, not separate technical sections. For each stage, include a flow sketch:
```
Flow: [Send Email] → [Wait 3 Days] → Decision(Opened/Not Opened)
      Opened → [Follow Up] → Merge → [Wait 1 Day] → End
      Not Opened → [SMS Reminder] → Merge
```

After adding all stages, update the YAML file and call `mcp__tas__open_file` so the user sees the complete diagram. Run the event count check (max 8 stages, 120 events/journey, 70 events/stage).

**Update the journey plan brief:** Add a `## Stage Details` section to the `[journey-name]-plan.md` file:

```markdown
## Stage Details

### Stage 1: [Stage Name]
- **Entry criteria:** [criteria]
- **Flow:** [flow sketch, e.g., Send Email → Wait 3 Days → Decision(Opened/Not Opened) → ...]
- **Activations:** [list of activation steps with channel and purpose]
- **Exit to next stage:** [milestone criteria]

### Stage 2: [Stage Name]
- **Entry criteria:** [criteria]
- **Flow:** [flow sketch]
- **Activations:** [list of activation steps]
- **Exit criteria:** [exit/completion criteria]

[Repeat for each stage]
```

Check off "Build out stage details" in Next Steps. Update the file and call `mcp__tas__open_file` to refresh it.

Then immediately proceed to Step 4.

#### Technical reference for journey design

**Step types**: `activation`, `wait`, `decision_point`, `ab_test`, `merge`, `end`, `jump`

**Segment rule format**:
```yaml
segment_name:
  description: Human-readable description
  rule:
    type: And
    conditions:
      - type: Value
        attribute: attribute_name
        operator:
          type: Equal | GreaterEqual | LessEqual | TimeWithinPast | ...
          value: "value"
```

**Activation format**:
```yaml
activation_name:
  name: Human-readable name
  connection: Connection Name
  all_columns: true
  run_after_journey_refresh: true
  connector_config:
    de_name: DataExtensionName
    data_operation: upsert
```

**Flow control rules**:
- Every branch must perform a different action (otherwise don't branch)
- Multiple paths must converge through a Merge step (not directly to End)
- A wait step must follow every activation step (via merge is OK)
- End step has no `next` and no `with`
- Decision point = fixed delay then check; condition wait = react immediately with timeout
- The "excluded" branch is the catch-all and needs no segment
- Condition wait paths must lead to different actions (otherwise use duration wait)

### Step 4: Journey Refinement

Now that the user can see the complete journey diagram, ask refinement questions to fine-tune the plan. Briefly explain: *"The journey structure is ready — now let's fine-tune the outcomes and pacing."*

Use a single AskUserQuestion with `layout: 'form'` containing 3 questions:

**Question 1 — Success metric** (single-select):
- Header: "Goal"
- Question: "What defines success for this journey?"
- Options:

  | Label | Description |
  |-------|-------------|
  | Conversion | Customer completes a purchase, signup, or key action |
  | Engagement | Customer opens, clicks, or interacts with messages |
  | Retention | Customer stays active and doesn't churn |
  | Revenue | Customer generates measurable revenue lift |

**Question 2 — Timing** (single-select):
- Header: "Pacing"
- Question: "How quickly should the journey move customers through stages?"
- defaultValue: "Moderate"
- Options:

  | Label | Description |
  |-------|-------------|
  | Gentle | Longer waits — 5-7 days between touchpoints |
  | Moderate | Standard pacing — 2-3 days between touchpoints |
  | Aggressive | Shorter waits — 1 day or less between touchpoints |

**Question 3 — Non-responder handling** (single-select):
- Header: "Fallback"
- Question: "What should happen to customers who don't respond to any message?"
- defaultValue: "Exit silently"
- Options:

  | Label | Description |
  |-------|-------------|
  | Exit silently | Remove from journey after final stage |
  | Last-chance message | Send one final message on a different channel, then exit |
  | Move to re-engagement | Transfer to a separate re-engagement flow |

After submission:
- Update the journey's goal segment to reflect the success metric.
- Adjust all wait step durations to match the selected pacing.
- Add exit criteria or a final fallback branch based on non-responder handling.
- Update the `.journey.yaml` and call `mcp__tas__open_file` to show the refined diagram.

**Update the journey plan brief:** Add a `## Journey Configuration` section to the `[journey-name]-plan.md` file:

```markdown
## Journey Configuration
- **Success Metric:** [selected metric] — [brief description of what this means for the journey]
- **Pacing:** [selected pacing] — [specific wait durations applied, e.g., "3-day waits between touchpoints"]
- **Non-Responder Handling:** [selected fallback] — [description of what happens]
```

Check off "Define success metrics and pacing" in Next Steps. Update the file and call `mcp__tas__open_file` to refresh it.

If "Other" is selected, ask one free-text follow-up for the metric definition.

### Step 5: Iterative Refinement Loop

After applying the refinements, ask the user what they'd like to improve next. Use AskUserQuestion (single-select) with LLM-generated options based on the current journey state. Always include "I'm done for now" as the last option.

Generate 2-3 contextual refinement suggestions based on what the journey currently lacks or could benefit from. Examples:
- If the journey has no A/B test steps → suggest "Add A/B testing to compare message variants"
- If only one channel is used → suggest "Add a secondary channel for non-responders"
- If there are no exit criteria → suggest "Add suppression rules for opted-out customers"
- If wait times are uniform → suggest "Vary timing based on customer engagement signals"
- If the journey has many stages → suggest "Simplify by merging similar stages"
- If no personalization is defined → suggest "Add personalization to activation messages"

**Always include** "I'm done for now" as the final option.

When the user selects a refinement option:
- Apply the change to the `.journey.yaml`
- **Update the `[journey-name]-plan.md`** to reflect the change in the relevant section (e.g., if an A/B test is added, update the affected stage in `## Stage Details`; if exit criteria are added, update `## Journey Configuration`)
- Call `mcp__tas__open_file` to show the updated diagram
- Ask the next round of refinement suggestions (loop back to this step)

When the user selects "I'm done for now":
- **Finalize the journey plan brief:** Update the `[journey-name]-plan.md` file:
  - Add a `## Design Decisions` section summarizing the key design rationale (branching logic, channel selection, pacing choices).
  - If any refinements were applied during the loop, ensure they are reflected in the relevant sections (e.g., A/B tests in Stage Details, suppression rules in Journey Configuration).
  - Replace the `## Next Steps` checklist with:

    ```markdown
    ## Status
    Journey plan complete. All stages, activations, and configurations are documented above.
    ```

  - Update the footer to:

    ```markdown
    ---
    *Journey plan finalized during guided planning flow. This is for demo purposes only — no changes have been made to any data or environment.*
    ```

  - Call `mcp__tas__open_file` to display the finalized brief.

- Send a closing message (no AskUserQuestion). Mention the journey plan and diagram are ready and can be refined or shared with their team. Note the full Treasure AI platform unifies CDP + journey orchestration + activation — invite them to talk to sales for the full experience. Keep warm and concise (3-4 sentences, no bullets/headers).


## Behavior

- Never suggest making changes to demo data. Never run `tdx` commands that create, modify, or delete resources. Frame all outputs as plans and previews.
- **Living brief** — generate the journey plan brief early (after Step 1) and update it progressively after each step. The user should always have a current artifact reflecting everything gathered so far. The brief complements the `.journey.yaml` — the YAML is the technical/visual artifact rendered as a diagram, while the brief is the human-readable strategic document.
- **Use AskUserQuestion with `layout: 'form'` for the setup step** — do NOT split journey type, audience, and channels into sequential single questions.
- Use the `defaultValue` and `defaultOtherText` fields to pre-fill sensible defaults so the user can accept or adjust.
- **Present the journey plan stage by stage**, not concept by concept. Users think about the customer experience (what happens at each stage), not CDP technical categories (segments vs. activations).
- **Explain concepts briefly inline** where they first appear — assume the user is new to CDP journey orchestration. Don't front-load all explanations before the plan.
- Keep journey plans practical — max 8 stages, clear decision logic, realistic wait times.
- When designing segments, use industry-appropriate attributes and realistic thresholds.
- This is PLANNING ONLY — do NOT run `tdx journey push` or any other `tdx` CLI commands.
