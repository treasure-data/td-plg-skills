# Goal: Find Customer Segment

**This flow is for demo purposes only.** We will NOT make any changes to the demo data or environment. No segments will be created, modified, or deleted. The output is purely a plan/preview document.

## Workflow

1. **Greet the user** — acknowledge their goal ("find customer segment") and output preference. Be concise and action-oriented. **Clearly state that this flow is for demo purposes only and no changes will be made to any data — the result is a plan preview only.**

2. **Ask industry, data source & segmentation goal** — before the form, briefly explain what a customer segment is and why the segment type matters (1–2 sentences).

   If industry and data source were already provided in the user's initial prompt (or a prior step), **omit those questions** — use those values directly and only ask the segmentation goal question. Otherwise, include all 3 questions in a single AskUserQuestion with `layout: 'form'`:

   **Question 1 — Industry** (single-select, skip if already known):
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

   **Question 2 — Data source** (single-select, skip if already known):
   - Header: "Data"
   - Question: "Which data source should we work with?"
   - Options:

     | Label | Description |
     |-------|-------------|
     | Synthetic data | Use pre-loaded sample datasets to explore features |
     | Upload my own data | Import CSV, JSON, or other files |

   **Question 3 — Segmentation goal** (single-select, `allowOther: true`):
   - Header: "Segment type"
   - Question: "What type of customer segment do you want to find?"
   - Suggest 3–4 segment types that are broadly applicable. Do not include a generic "Custom" or freeform option in the suggested list — `allowOther` already provides an "Other" option for that.

   After the user answers:
   - Tailor terminology, examples, and KPIs to their industry for the rest of the session.
   - If synthetic data: use pre-loaded sample datasets to explore features.
   - If upload: remind the user to upload via the "+" icon in the chat window and wait for the file attachment before proceeding. If data is already uploaded, use that. Avoid personal or confidential data.

3. **Generate initial segment brief** — based on the user's answers, generate a markdown file named `[segment-name]-brief.md` (auto-generated from goal + industry, e.g., `retail-high-value-brief.md`). Write to the working directory and open with `mcp__tdx-studio__open_file`. Tell the user: "I've created an initial segment brief. We'll fill in the details as we explore your data."

   Initial brief template:

   ```markdown
   # [Segment Name] — Segment Brief

   ## Overview
   - **Goal:** [selected segmentation goal]
   - **Industry:** [selected industry]
   - **Data Source:** [selected data source]

   ## Industry Context
   [1–2 sentences on industry-specific segmentation context]

   ## Direction
   [2–3 sentences outlining the strategic direction based on goal + industry]

   ## Next Steps
   - [ ] Explore available data
   - [ ] Define segment criteria
   - [ ] Estimate segment size

   ---
   *Draft generated during segment planning. This document will be updated as we refine the criteria.*
   ```

   This brief is a **living document** — update it progressively in subsequent steps as more information is gathered.

4. **Explore data + suggest criteria** — based on the segmentation goal, explore the available data to understand what columns and distributions exist. Then suggest 1–3 concrete segment criteria **grounded in the actual data** (e.g., "Customers with 3+ orders" only if an `order_count` column exists with meaningful distribution). Present:
   - A brief summary of the relevant data (tables, key columns, row counts)
   - 1–3 suggested segment definitions with rule logic (attributes, operators, values) derived from the data
   - Any data quality notes the user should be aware of
   Use AskUserQuestion to let the user pick a suggestion or ask to adjust. Do not add a "Custom audience" or similar freeform option — `allowOther` already provides that. Confirm the final criteria before proceeding.

   **Update the brief:** add a `## Data Summary` section with tables, columns, row counts, and data quality notes. Check off "Explore available data" in Next Steps.

5. **Build segment plan + verify** — construct the full segment definition using confirmed criteria. Present:
   - Final criteria and rule logic
   - Estimated segment size
   - Key characteristics / profile summary
   - Data sources and columns used
   Use AskUserQuestion (single-select) with options: "Looks good, proceed", "I'd like to adjust". If the user wants adjustments, ask what to change and loop back.

   **Update the brief:** add `## Segment Definition` with final criteria, rule logic, estimated size, profile summary, and data sources. Check off "Define segment criteria" and "Estimate segment size".

6. **Deliver results + suggest activation ideas** — generate a CSV file with the segment member list and key attributes, and offer it for download. Proactively suggest activation ideas (campaigns, journeys) for the segment, but clarify these are suggestions for a real implementation — not actions being taken now.

   **Update the brief (final):** add `## Activation Ideas` with suggested campaigns/journeys. Replace the Next Steps checklist with a completion note. Update the footer to mark the brief as finalized. Open the completed brief with `mcp__tdx-studio__open_file`.

## Behavior

- **This is a demo-only flow.** Never suggest making changes to demo data. Never run `tdx` commands that create, modify, or delete resources. Frame all outputs as plans and previews.
- **Living brief** — generate the segment brief early and update it progressively after each step. The user should always have a current artifact reflecting everything gathered so far.
- **Explain before asking** — before each question or decision point, briefly explain the concept and why it matters. Assume the user is new to customer segmentation. Keep explanations concise (1–2 sentences).
- **Data-first criteria** — always explore available data before suggesting segment criteria. Every suggestion must reference columns and distributions that actually exist. Do not suggest criteria that the data cannot support.
- Ask one question at a time. Do not overwhelm the user.
- Use the AskUserQuestion tool for all structured questions — do NOT ask as plain text.
- When presenting segment results, always include the segment size and a brief profile summary.
