# Goal: Campaign Planning

Plan an email campaign through 5 streamlined steps using FormCard forms, then review and generate artifacts. This is PLANNING ONLY — produce campaign plan artifacts. Do NOT run `tdx` commands, push templates, or deploy campaigns.

**This flow is for demo purposes only.** We will NOT make any changes to the demo data or environment. No campaigns, templates, or segments will be created, modified, or deleted. The output is purely a plan/preview document.

## Workflow

1. **Greet the user** — acknowledge their goal ("campaign planning"). Be concise and action-oriented. **Clearly state that this flow is for demo purposes only and no changes will be made to any data — the result is a plan preview only.**

2. **Ask industry & data source** — use a single AskUserQuestion with `layout: 'form'` containing 2 questions:

   **Question 1 — Industry** (single-select):
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

   **Question 2 — Data source** (single-select):
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

3. **Walk the user through 5 streamlined steps (plus review and artifact generation).** Each step uses a single AskUserQuestion with `layout: 'form'` so the user sees all related questions at once and can adjust pre-filled defaults.

### Step 1: Goal & Success Criteria

Briefly explain what a campaign goal and KPI are (1–2 sentences): *"A campaign goal defines what you want the email to achieve. A KPI (Key Performance Indicator) is the metric you'll use to measure success."*

Use a single AskUserQuestion with `layout: 'form'` containing 3 questions:

**Question 1 — Campaign goal** (single-select):
- Header: "Goal"
- Question: "What is the primary goal of this email campaign?"
- Options:

  | Label | Description |
  |-------|-------------|
  | Drive first purchase | Convert new subscribers or leads into first-time buyers |
  | Re-engage lapsed users | Win back customers who haven't interacted recently |
  | Promote a product or offer | Highlight a specific product, sale, or limited-time offer |
  | Nurture leads | Educate and build trust with prospects over time |
  | Announce news or event | Share a product launch, event, or company update |

**Question 2 — Success KPI** (single-select):
- Header: "KPI"
- Question: "How will you measure success?"
- defaultValue: "Conversion rate"
- Options:

  | Label | Description |
  |-------|-------------|
  | Conversion rate | % of recipients who take the desired action |
  | Open rate | % of recipients who open the email |
  | Click-through rate | % of recipients who click a link in the email |
  | Revenue per email | Total revenue generated / emails sent |
  | Engagement rate | Overall interaction rate (opens + clicks + replies) |

**Question 3 — KPI target** (free-text):
- Header: "Target"
- Question: "What is your target for this KPI? (e.g., '15% conversion rate', '25% open rate')"
- options: [] (empty — renders as text input)
- allowOther: true
- defaultOtherText: "Establish a baseline"

After submission, interpret the goal+KPI combination to infer the most relevant measurement approach. If "Other" is selected, ask one free-text follow-up for the goal description.

### Step 2: Generate Campaign Plan Draft

Based on the goal, KPI, target, industry, and data source collected so far, generate an initial campaign plan as a markdown file named `[campaign-name]-plan.md`. The campaign name should be auto-generated from the goal + industry (e.g., `retail-first-purchase-plan.md`).

The initial `.md` file should contain:

```markdown
# [Campaign Name] — Campaign Plan

## Campaign Overview
- **Goal:** [selected goal]
- **KPI:** [selected KPI]
- **Target:** [target value]

## Industry Context
- **Industry:** [selected industry]
- **Data Source:** [selected data source]
- **Key considerations:** [1-2 sentences on industry-specific email marketing context]

## Campaign Direction
[2-3 sentences outlining the strategic direction based on goal + industry]

## Next Steps
- [ ] Define audience segments
- [ ] Design email template
- [ ] Set campaign schedule

---
*Draft generated during campaign planning. This document will be updated with full details after all steps are complete.*
```

Write this file to the working directory and call `mcp__tdx-studio__open_file` to display it in the artifact panel. Briefly tell the user: "I've created an initial campaign plan draft. We'll fill in the details as we go through the remaining steps."

Then proceed to Step 3 (Audience Segments).

### Step 3: Audience Segments

Explore the available data based on the user's industry and data source. Present:
- A brief explanation of parent and child segments (2–3 sentences): *"A parent segment is your master customer database — it contains all the attributes we can use to target people. A child segment is a specific audience slice carved from the parent using rules and filters."*
- A data summary showing what tables/columns are available

Then use a single AskUserQuestion with `layout: 'form'` containing 2 questions:

**Question 1 — Parent segment type** (single-select):
- Header: "Data foundation"
- Question: "Which type of customer data will power this campaign's targeting?"
- defaultValue: set to the "Full profile" option for the user's industry
- Options tailored to industry:

For Retail / D2C:
  | Label | Description |
  |-------|-------------|
  | Purchase & transaction data | Order history, AOV, recency, frequency |
  | Engagement & browsing data | Page views, email opens, app sessions |
  | Full customer profile | Combined purchase + engagement + demographics |

For B2B Tech:
  | Label | Description |
  |-------|-------------|
  | Product usage data | Feature adoption, login frequency, usage depth |
  | Account & firmographic data | Company size, industry, plan tier |
  | Full account profile | Combined usage + firmographic + engagement |

For Travel:
  | Label | Description |
  |-------|-------------|
  | Booking & travel data | Trip history, booking value, destinations |
  | Loyalty & engagement data | Tier status, points balance, app usage |
  | Full traveler profile | Combined booking + loyalty + demographics |

For CPG:
  | Label | Description |
  |-------|-------------|
  | Purchase & consumption data | Purchase frequency, basket size, brand loyalty |
  | Promotional response data | Coupon redemption, promo engagement, price sensitivity |
  | Full shopper profile | Combined purchase + promo + demographic data |

For Automotive:
  | Label | Description |
  |-------|-------------|
  | Sales & lead data | Lead status, test drives, purchase history, vehicle owned |
  | Service & engagement data | Service visits, recall response, digital engagement |
  | Full customer profile | Combined sales + service + engagement data |

For Media:
  | Label | Description |
  |-------|-------------|
  | Content consumption data | Watch/read history, time spent, genre preferences |
  | Subscription & billing data | Plan tier, tenure, payment status |
  | Full audience profile | Combined consumption + subscription + engagement |

**Question 2 — Target audience** (multi-select):
- Header: "Audience"
- Question: "Which audience segment(s) should receive this campaign?"
- Suggest 1–3 child segments with rule definitions **grounded in the actual data** (e.g., "Customers with 3+ orders" only if an `order_count` column exists). Do not add a "Custom audience" or similar freeform option — rely on the built-in "Other" option instead.

**Conditional behavior by data source:**
- **Synthetic data**: Reference concrete tables and columns from the sample datasets.
- **Upload my own data**: Inspect the uploaded file's columns and map them to segment attributes. If no file is uploaded yet, remind the user to upload via the "+" icon and wait.

If "Other" is selected, ask one free-text follow-up for their targeting criteria.

### Step 4: Email Template

Briefly explain what an email template plan is (1–2 sentences): *"The email template defines the content, structure, and personalization of your email. We'll set up the subject line, merge tags, layout sections, and any conditional content."*

Based on the campaign goal, industry, and target segment, suggest 2–3 subject line options:

| Option | Subject Line | Preview Text | Why |
|--------|-------------|--------------|-----|
| A | [industry-tailored suggestion] | [preview text] | [rationale] |
| B | [industry-tailored suggestion] | [preview text] | [rationale] |
| C | [industry-tailored suggestion] | [preview text] | [rationale] |

Then use a single AskUserQuestion with `layout: 'form'` containing 4 questions:

**Question 1 — Subject line** (single-select):
- Header: "Subject"
- Question: "Which subject line direction do you prefer?"
- Options: A, B, C (from suggestions above), and "Write my own"

**Question 2 — Personalization variables** (multi-select):
- Header: "Variables"
- Question: "Which personalization variables should we include?"
- defaultValue: ["First name"]
- Options: industry-relevant merge tags based on the parent segment. Examples:

  | Label | Description |
  |-------|-------------|
  | First name | Greeting personalization |
  | Last product | Reference their most recent purchase |
  | Loyalty tier | Tier-specific messaging |

  Tailor to industry and data source. Do not add a "Custom variable" or similar freeform option — rely on "Other" instead.

**Question 3 — Email sections** (multi-select):
- Header: "Layout"
- Question: "What sections should the email include?"
- defaultValue: ["Header with logo", "Hero image + headline", "Body text", "CTA button", "Footer"]
- Options:

  | Label | Description |
  |-------|-------------|
  | Header with logo | Brand logo and optional navigation links |
  | Hero image + headline | Large visual with primary message |
  | Body text | Main content block with supporting copy |
  | Product showcase | Grid or list of featured products or items |
  | CTA button | Primary call-to-action button |
  | Social proof | Testimonials, ratings, or trust badges |
  | Secondary CTA | Additional action link or button |
  | Footer | Unsubscribe link, company info, legal text |

**Question 4 — Conditional content** (single-select):
- Header: "Conditions"
- Question: "Should any email sections show different content based on customer attributes?"
- defaultValue: "No conditional content"
- Options:

  | Label | Description |
  |-------|-------------|
  | No conditional content | Same email content for all recipients |
  | Product recommendations | Show different products based on purchase history or preferences |
  | Messaging by segment tier | Different headline/body for VIP vs. regular customers |
  | Regional variations | Different offers or content by geography |

If "Write my own" (subject) or "Other" (conditions) is selected, ask one free-text follow-up for each.

### Step 5: Schedule

Briefly explain scheduling considerations (1–2 sentences): *"Campaign scheduling determines when your email goes out. Consider your audience's time zone, optimal send times for your industry, and whether this is a one-time or recurring campaign."*

Use a single AskUserQuestion with `layout: 'form'` containing 3 questions:

**Question 1 — Send type** (single-select):
- Header: "Frequency"
- Question: "What type of send schedule do you need?"
- Options:

  | Label | Description |
  |-------|-------------|
  | One-time send | Send once at a specific date and time |
  | Recurring (daily) | Send daily to new segment members |
  | Recurring (weekly) | Send weekly on a specific day |
  | Recurring (monthly) | Send monthly on a specific date |
  | Triggered | Send when a customer enters the target segment |

**Question 2 — Send time** (single-select):
- Header: "Time"
- Question: "What time of day should the email be sent?"
- defaultValue: pick the industry-appropriate default:
  - B2B Tech → "Morning (9–10 AM)"
  - Retail/D2C/CPG → "Midday (12–1 PM)"
  - Automotive → "Afternoon (3–4 PM)"
  - Media/Travel → "Evening (7–8 PM)"
- Options:

  | Label | Description |
  |-------|-------------|
  | Morning (9–10 AM) | Best for B2B, news, and informational emails |
  | Midday (12–1 PM) | Good for retail promotions and lunch-break browsing |
  | Afternoon (3–4 PM) | Effective for D2C and lifestyle brands |
  | Evening (7–8 PM) | Strong for entertainment, travel, and leisure |
  | Custom time | Specify your preferred send time |

**Question 3 — Campaign duration** (single-select):
- Header: "Duration"
- Question: "How long should this campaign run?"
- defaultValue: "Ongoing (no end date)"
- Options:

  | Label | Description |
  |-------|-------------|
  | 1 week | Short promotional burst |
  | 2 weeks | Standard campaign window |
  | 1 month | Extended campaign run |
  | 3 months | Quarterly campaign |
  | Ongoing (no end date) | Runs until manually stopped |

### Step 6: Review

Present a structured summary:

**Campaign Overview**
- Campaign name (auto-generated from goal + industry + segment)
- Campaign goal
- Success KPI and target

**Audience**
- Parent segment: name and key attributes
- Target child segment(s): name(s) with rule summaries
- Estimated audience description

**Email Template**
- Subject line and preview text
- Personalization variables
- Email structure (list of sections)
- Conditional content rules

**Schedule**
- Send type (one-time / recurring / triggered)
- Send time
- Duration (if recurring)

**Campaign Settings**
- UTM parameters (auto-suggested):
  - utm_source: treasure_data
  - utm_medium: email
  - utm_campaign: [campaign-name-slug]
  - utm_content: [variant identifier]

Use AskUserQuestion:
- Question: "Does this campaign plan look good? What would you like to change?"
- Single-select:

  | Label | Description |
  |-------|-------------|
  | Looks good — generate the artifacts | Proceed to output generation |
  | Change the audience segments | Go back to segment planning |
  | Change the email template | Go back to template planning |
  | Change the schedule | Go back to schedule planning |
  | Change the campaign goal | Start over with a new goal |

If the user requests changes, return to the appropriate step. Otherwise proceed to artifact generation.

### Step 7: Generate Final Artifacts

Generate two outputs:

#### Artifact 1: HTML Email Template

**[campaign-name]-template.html** — a complete, responsive HTML email:
- Use table-based layout for email client compatibility
- Include all sections the user selected in Step 4
- Insert merge tag placeholders as `{{profile.variable_name}}`
- Include conditional content blocks using Liquid syntax where applicable:
  ```html
  {% if profile.attribute == 'value' %}
    <p>Conditional content here</p>
  {% else %}
    <p>Default content here</p>
  {% endif %}
  ```
- Apply industry-appropriate styling (colors, fonts, imagery descriptions)
- Include responsive meta tags and email-client-safe inline CSS
- Add alt text placeholders for images
- Include unsubscribe link in footer: `{{unsubscribe_url}}`
- Use placeholder image URLs (e.g., `https://via.placeholder.com/600x300`) and realistic copy based on the campaign goal and industry

The HTML should be a complete, working email template — not a skeleton.

#### Artifact 2: Updated Campaign Plan (.md)

Update the **[campaign-name]-plan.md** file created in Step 2 with all information gathered during the flow. The final `.md` should contain:

```markdown
# [Campaign Name] — Campaign Plan

## Campaign Overview
- **Goal:** [selected goal]
- **KPI:** [selected KPI]
- **Target:** [target value]

## Industry Context
- **Industry:** [selected industry]
- **Data Source:** [selected data source]
- **Key considerations:** [industry-specific context]

## Campaign Direction
[Strategic direction summary from Step 2, refined with full context]

## Audience
### Parent Segment
- **Type:** [selected parent segment type]
- **Data foundation:** [description]
- **Key attributes:** [list of attributes]

### Target Segments
For each selected child segment:
- **[Segment name]:** [description]
  - Rule: [rule summary, e.g., "order_count >= 3 AND last_order_date within 90 days"]

## Email Template
- **Subject line:** [selected subject line]
- **Preview text:** [preview text]
- **Personalization variables:** [list of merge tags with defaults]
- **Layout sections:** [list of selected sections]
- **Conditional content:** [type and description, or "None"]
- **HTML file:** `./[campaign-name]-template.html`

## Schedule
- **Send type:** [one-time / recurring (daily/weekly/monthly) / triggered]
- **Send time:** [selected time with timezone]
- **Duration:** [selected duration]

## Campaign Settings
- **UTM Parameters:**
  - utm_source: treasure_data
  - utm_medium: email
  - utm_campaign: [campaign-slug]
  - utm_content: [variant-id]

## Optimization Suggestions
- **A/B testing:** Test subject lines A vs B to optimize open rates
- **Suppression rules:** Exclude recently emailed (< 7 days), unsubscribed, and hard-bounced addresses
- **Preheader optimization:** [specific suggestion based on subject line]
- **Dynamic content:** [suggestion based on conditional content selection]
- **Follow-up campaign:** Re-send to non-openers after 3 days with alternate subject line

---
*Campaign plan finalized during guided planning flow. This is for demo purposes only — no changes have been made to any data or environment.*
```

#### Delivery

Write the updated `.md` file and the HTML file to the working directory.

Use `mcp__tdx-studio__open_file` to display the HTML template in the artifact panel for visual review.

Additionally, deliver in the user's preferred output format (from `[User Preferences]`):
- **Interactive chart**: generate an HTML summary page showing the complete campaign plan visually (overview, audience flow, template preview, timeline). Open with `mcp__tdx-studio__open_file`.
- **Data table**: present the plan as structured tables (audience, template, schedule, settings sections).
- **Written summary**: narrative walkthrough of the complete campaign with rationale for each decision.

These are plan artifacts — do NOT run `tdx` commands or push to any server.

Remind the user at the end: *"This campaign plan is for demo purposes only. No changes have been made to any data or environment. In a real implementation, you would use the appropriate `tdx` commands to deploy templates and activate campaigns."*

## Behavior

- **This is a demo-only flow.** Never suggest making changes to demo data. Never run `tdx` commands that create, modify, or delete resources. Frame all outputs as plans and previews.
- **Use AskUserQuestion with `layout: 'form'` for multi-question steps** — do NOT split into sequential single questions. Each step should be a single FormCard interaction.
- Use the `defaultValue` and `defaultOtherText` fields to pre-fill sensible defaults so the user can accept or adjust.
- **Explain every concept briefly before the form** — assume the user is new to email marketing campaigns. Use plain language first, then show the technical representation.
- Keep campaign plans realistic — suggest industry-appropriate send times, realistic KPI targets, and sensible segment rules.
- When designing segments, use industry-appropriate attributes and realistic thresholds from the selected data source.
- When generating HTML email templates, ensure email-client compatibility (table-based layout, inline CSS, email-safe fonts).
- Include optimization suggestions in the final campaign plan .md file:
  - A/B test subject lines or send times
  - Suppression rules (recently emailed, unsubscribed, bounced)
  - Preheader text optimization
  - Dynamic content blocks for different audience segments
  - Follow-up campaign for non-openers
- This is PLANNING ONLY — do NOT run `tdx engage` or any other `tdx` CLI commands. Do NOT push templates or deploy campaigns.
