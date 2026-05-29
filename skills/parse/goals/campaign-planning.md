# Goal: Campaign Planning

Plan an email campaign through 4 streamlined steps using FormCard forms, then review and generate artifacts. This is PLANNING ONLY — produce campaign plan artifacts. Do NOT run `tdx` commands, push templates, or deploy campaigns.

**This flow is for demo purposes only.** We will NOT make any changes to the demo data or environment. No campaigns, templates, or segments will be created, modified, or deleted. The output is purely a plan/preview document.

## Workflow

1. **Greet the user** — write a brief, natural greeting that reflects what the user actually asked for. Mirror their language and energy. Be concise and action-oriented. Do not use a canned phrase — tailor the greeting to their specific request.

2. **Ask industry, data source & campaign goal** — before the form, briefly explain what email campaign planning involves and why the goal matters (1–2 sentences).

   If industry, data source, and/or campaign goal were already provided in the user's initial prompt (or a prior step), **omit those questions** — use those values directly and only ask the remaining questions. If all 3 are already known, skip this step entirely and proceed to Step 1. Otherwise, include only the missing questions in a single AskUserQuestion with `layout: 'form'`:

   **Question 1 — Campaign goal** (single-select, skip if already known):
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

   **Question 2 — Industry** (single-select, skip if already known):
   - Header: "Industry"
   - Question: "What industry are you in?"
   - Options:

     | Label | Description |
     |-------|-------------|
     | Retail | |
     | Automotive | |
     | CPG | |
     | Media | |

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
   - If synthetic data: use pre-loaded sample datasets. Briefly describe the data tables the user has access to — list each table name with a one-line description of what it contains. Keep it concise (no column details yet).
   - If upload: remind the user to upload via the "+" icon in the chat window and wait for the file attachment before proceeding. If data is already uploaded, use that. Avoid personal or confidential data.
   - If "Other" is selected for the campaign goal, ask one free-text follow-up for the goal description.

3. **Walk the user through 4 streamlined steps (plus review and artifact generation).** Each step uses a single AskUserQuestion with `layout: 'form'` so the user sees all related questions at once and can adjust pre-filled defaults.

### Step 1: Generate Campaign Plan Draft

Based on the goal, industry, and data source collected so far, generate an initial campaign plan as a markdown file named `[campaign-name]-plan.md`. The campaign name should be auto-generated from the goal + industry (e.g., `retail-first-purchase-plan.md`).

**Auto-generate the success KPI and target:** Based on the selected campaign goal and industry, automatically determine the most appropriate success KPI (e.g., conversion rate, open rate, CTR, revenue per email, engagement rate) and a realistic target value. Do not ask the user — infer these from the goal+industry combination.

The initial `.md` file should contain:

```markdown
# [Campaign Name] — Campaign Plan

## Next Steps
- [ ] Define audience segments
- [ ] Design email template

## Campaign Overview
- **Goal:** [selected goal]
- **KPI:** [auto-generated KPI based on goal + industry]
- **Target:** [auto-generated realistic target value]

## Industry Context
- **Industry:** [selected industry]
- **Data Source:** [selected data source]
- **Key considerations:** [1-2 sentences on industry-specific email marketing context]

## Campaign Direction
[2-3 sentences outlining the strategic direction based on goal + industry]

---
*Draft generated during campaign planning. This document will be updated with full details after all steps are complete.*
```

Write this file to the working directory and call `mcp__tdx-studio__open_file` to display it in the artifact panel. Briefly tell the user: "I've created an initial campaign plan draft. We'll fill in the details as we go through the remaining steps."

Then proceed to Step 2 (Audience Segments).

### Step 2: Audience Segments

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

### Step 3: Email Template

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

### Step 4: Review

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
  | Change the campaign goal | Start over with a new goal |

If the user requests changes, return to the appropriate step. Otherwise proceed to artifact generation.

### Step 5: Generate Final Artifacts

Generate two outputs:

#### Artifact 1: HTML Email Template

**[campaign-name]-template.html** — a complete, responsive HTML email:
- Use table-based layout for email client compatibility
- Include all sections the user selected in Step 3
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

Update the **[campaign-name]-plan.md** file created in Step 1 with all information gathered during the flow. The final `.md` should contain:

```markdown
# [Campaign Name] — Campaign Plan

## Campaign Overview
- **Goal:** [selected goal]
- **KPI:** [auto-generated KPI based on goal + industry]
- **Target:** [auto-generated realistic target value]

## Industry Context
- **Industry:** [selected industry]
- **Data Source:** [selected data source]
- **Key considerations:** [industry-specific context]

## Campaign Direction
[Strategic direction summary from Step 1, refined with full context]

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
- **Interactive chart**: generate an HTML summary page showing the complete campaign plan visually (overview, audience flow, template preview). Open with `mcp__tdx-studio__open_file`.
- **Data table**: present the plan as structured tables (audience, template, settings sections).
- **Written summary**: narrative walkthrough of the complete campaign with rationale for each decision.

These are plan artifacts — do NOT run `tdx` commands or push to any server.

Remind the user at the end: *"This campaign plan is for demo purposes only. No changes have been made to any data or environment. In a real implementation, you would use the appropriate `tdx` commands to deploy templates and activate campaigns."*

## Behavior

- Never suggest making changes to demo data. Never run `tdx` commands that create, modify, or delete resources. Frame all outputs as plans and previews.
- **Use AskUserQuestion with `layout: 'form'` for multi-question steps** — do NOT split into sequential single questions. Each step should be a single FormCard interaction.
- Use the `defaultValue` and `defaultOtherText` fields to pre-fill sensible defaults so the user can accept or adjust.
- **Explain every concept briefly before the form** — assume the user is new to email marketing campaigns. Use plain language first, then show the technical representation.
- Keep campaign plans realistic — suggest realistic KPI targets and sensible segment rules.
- When designing segments, use industry-appropriate attributes and realistic thresholds from the selected data source.
- When generating HTML email templates, ensure email-client compatibility (table-based layout, inline CSS, email-safe fonts).
- Include optimization suggestions in the final campaign plan .md file:
  - A/B test subject lines
  - Suppression rules (recently emailed, unsubscribed, bounced)
  - Preheader text optimization
  - Dynamic content blocks for different audience segments
  - Follow-up campaign for non-openers
- **Generate small, minimal HTML template files** that can be produced quickly — avoid bloated markup, excessive inline styles, or placeholder content that inflates file size. Keep the template lean and focused.
- This is PLANNING ONLY — do NOT run `tdx engage` or any other `tdx` CLI commands. Do NOT push templates or deploy campaigns.
