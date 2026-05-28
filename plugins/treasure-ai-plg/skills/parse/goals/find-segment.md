# Goal: Find Customer Segment

**This flow is for demo purposes only.** We will NOT make any changes to the demo data or environment. No segments will be created, modified, or deleted. The output is purely a plan/preview document.

## Workflow

1. **Greet the user** — acknowledge their goal ("find customer segment") and output preference. Be concise and action-oriented. **Clearly state that this flow is for demo purposes only and no changes will be made to any data — the result is a plan preview only.**

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
   - If synthetic data: use the `treasurebikes` database — a sample dataset with customer, order, and product data.
   - If upload: remind the user to upload via the "+" icon in the chat window and wait for the file attachment before proceeding. If data is already uploaded, use that. Avoid personal or confidential data.

3. **Ask segmentation goal** — explain what a customer segment is and why the type matters (different types require different data and produce different results). Based on the user's industry, suggest 3–4 segment types that are most relevant and actionable. Do not include a generic "Custom" or freeform option in the suggested list — `allowOther` already provides an "Other" option for that. Use AskUserQuestion (single-select, allowOther: true).

4. **Explore data + suggest criteria** — based on the segmentation goal, explore the available data to understand what columns and distributions exist. Then suggest 1–3 concrete segment criteria **grounded in the actual data** (e.g., "Customers with 3+ orders" only if an `order_count` column exists with meaningful distribution). Present:
   - A brief summary of the relevant data (tables, key columns, row counts)
   - 1–3 suggested segment definitions with rule logic (attributes, operators, values) derived from the data
   - Any data quality notes the user should be aware of
   Use AskUserQuestion to let the user pick a suggestion or ask to adjust. Do not add a "Custom audience" or similar freeform option — `allowOther` already provides that. Confirm the final criteria before proceeding.

5. **Build segment plan + verify** — construct the full segment definition using confirmed criteria. Present:
   - Final criteria and rule logic
   - Estimated segment size
   - Key characteristics / profile summary
   - Data sources and columns used
   Use AskUserQuestion (single-select) with options: "Looks good, proceed", "I'd like to adjust". If the user wants adjustments, ask what to change and loop back.

6. **Deliver results + suggest activation ideas** — present in the user's preferred output format:
   - Interactive chart: distribution visualization via HTML, call `mcp__tas__open_file`
   - Data table: segment member list with key attributes, offer CSV/XLSX download
   - Written summary: segment profile with actionable insights
   After delivering, proactively suggest activation ideas (campaigns, journeys) for the segment, but clarify these are suggestions for a real implementation — not actions being taken now.

## Behavior

- **This is a demo-only flow.** Never suggest making changes to demo data. Never run `tdx` commands that create, modify, or delete resources. Frame all outputs as plans and previews.
- **Explain before asking** — before each question or decision point, briefly explain the concept and why it matters. Assume the user is new to customer segmentation. Keep explanations concise (1–2 sentences).
- **Data-first criteria** — always explore available data before suggesting segment criteria. Every suggestion must reference columns and distributions that actually exist. Do not suggest criteria that the data cannot support.
- Ask one question at a time. Do not overwhelm the user.
- Use the AskUserQuestion tool for all structured questions — do NOT ask as plain text.
- When presenting segment results, always include the segment size and a brief profile summary.
