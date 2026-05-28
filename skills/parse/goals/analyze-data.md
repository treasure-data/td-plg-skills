# Goal: Analyze Data

## Workflow

1. **Greet the user** — acknowledge their goal ("analyze data") and output preference. Be concise and action-oriented.

2. **Ask industry, data source & analysis purpose** — if industry, data source, or analysis purpose were already provided in the user's initial prompt (or a prior step), **omit those questions** — use those values directly and only ask the remaining questions. If the user's stated purpose is vague, ask a clarifying question about the analysis goal instead of showing the full options. If all three are known, skip the form entirely.

   Otherwise, use a single AskUserQuestion with `layout: 'form'` containing up to 3 questions (omitting any already known):

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

   **Question 3 — Analysis purpose** (single-select):
   - Header: "Purpose"
   - Question: "What would you like to learn from your data?"
   - Options:

     | Label | Description |
     |-------|-------------|
     | Understand my customers | Who they are, what they do, and why |
     | Track performance | Measure KPIs, trends, or campaign results |
     | Find opportunities | Discover growth areas, segments, or patterns |

   If the user selects "Other" for the analysis purpose, ask a free-text follow-up using AskUserQuestion.

   After the user answers:
   - Tailor terminology, examples, and KPIs to their industry for the rest of the session.
   - If synthetic data: use pre-loaded sample datasets to explore features.
   - If upload: remind the user to upload via the "+" icon in the chat window and wait for the file attachment before proceeding. If data is already uploaded, use that. Avoid personal or confidential data.
   - Use the selected purpose to guide data exploration (step 3), analysis approach suggestions (step 3), and result framing (step 6).

3. **Explore data + suggest analysis approaches** — briefly explain what data is available and why it matters before showing tables. Focus the exploration on tables and columns most relevant to the user's analysis purpose. Based on the data source:
   - Synthetic data: list available tables in the database and describe their schemas (table names, key columns). Skip row counts, column distributions, and data quality checks — defer those to the analysis execution step.
   - Upload my own data: wait for file upload, then inspect the file structure (columns, types). Skip row counts.

   Then, based on the user's analysis purpose, industry, and the actual data available, suggest 2–3 concrete analysis approaches **grounded in the data** (e.g., only suggest basket analysis if transaction-level purchase data exists). Explain what an analysis approach is and why you're recommending these specific ones. Use AskUserQuestion to let the user pick an approach or describe their own. Examples by industry:
   - Retail: basket analysis, customer lifetime value, seasonal trends
   - CPG: brand switching, promotion lift, channel mix
   - Travel: booking funnel, destination affinity, loyalty tier analysis
   - Automotive: lead-to-sale conversion, service retention, model preference
   - Media: content engagement, churn prediction, audience overlap
   - D2C: cohort retention, acquisition channel ROI, repeat purchase rate
   - B2B Tech: pipeline velocity, product adoption, expansion revenue

   **Generate analysis brief:** after the user selects an approach, generate a markdown file named `[analysis-name]-brief.md` (auto-generated from purpose + industry, e.g., `retail-customer-ltv-brief.md`). Write to the working directory and open with `mcp__tdx-studio__open_file`. Tell the user: "I've created an initial analysis brief. We'll update it with results as we go."

   Initial brief template:

   ```markdown
   # [Analysis Name] — Analysis Brief

   ## Next Steps
   - [ ] Execute analysis queries
   - [ ] Summarize findings

   ## Overview
   - **Purpose:** [selected analysis purpose]
   - **Industry:** [selected industry]
   - **Data Source:** [selected data source]
   - **Approach:** [selected analysis approach]

   ## Industry Context
   [1–2 sentences on industry-specific analysis context]

   ## Data Schema
   ### [Table/File Name]
   | Column | Type | Description |
   |--------|------|-------------|
   | ... | ... | ... |

   [Repeat for each relevant table/file]

   ## Analysis Direction
   [2–3 sentences outlining the analytical direction based on purpose + approach + data]

   ---
   *Draft generated during analysis planning. This document will be updated with results.*
   ```

   This brief is a **living document** — update it progressively in subsequent steps as more information is gathered.

4. **Execute analysis** — before running analysis queries, check row counts for the tables being used to inform result assessment. Then run queries, compute metrics, and build the output.

   **Update the brief:** add a `## Results` section with the computed metrics, key data points, and any intermediate findings. Check off "Execute analysis queries" in Next Steps.

5. **Ask output format** — before delivering results, use AskUserQuestion (single-select) to ask how the user wants to see the results:

   - Header: "Output"
   - Question: "How would you like to see the results?"
   - Options:

     | Label | Description |
     |-------|-------------|
     | Interactive chart | Visual HTML dashboard with hover tooltips |
     | Data table | Written summary with key takeaways and data table |

6. **Deliver results** — present the analysis results based on the user's output choice.

   **If interactive chart:** generate `[analysis-name]-results.html` — a self-contained HTML page (inline CSS + JS, no external dependencies). Keep the file small and quick to generate — simple inline SVG, minimal JS for hover tooltips. Populate with actual query results, not placeholder data. Match the chart type to the analysis approach:

   | Approach | Chart |
   |----------|-------|
   | Basket analysis | Network graph (SVG nodes + edges), hover for lift/support |
   | CLV | Histogram with percentile markers, hover for count |
   | Cohort retention | Heatmap grid (rows=cohorts, cols=periods), hover for retention % |
   | Seasonal trends | Multi-line time series, crosshair hover |
   | Pipeline velocity | Horizontal funnel, hover for conversion rate + avg days |
   | Other | Bar or line chart with hover tooltips |

   Include a "Key Takeaways" section below the chart as styled HTML text. Write the file and open with `mcp__tdx-studio__open_file`.

   **If data table:** present the analysis results as a written summary with key takeaways. Include a data table when the analysis benefits from showing specific numbers.

   **Update the brief (final):** add a `## Key Takeaways` section with the summary findings and actionable recommendations. Replace the Next Steps checklist with a completion note. Update the footer to mark the brief as finalized. Open the completed brief with `mcp__tdx-studio__open_file`.

## Behavior

- **Living brief** — generate the analysis brief early and update it progressively after each step. The user should always have a current artifact reflecting everything gathered so far.
- **Explain before asking** — before each question or decision point, briefly explain the concept and why it matters. Assume the user is new to data analysis. Keep explanations concise (1–2 sentences).
- Ask one question at a time. Do not overwhelm the user.
- Proactively suggest next steps after each deliverable.
- If the user's request doesn't make sense with the available data, suggest alternatives rather than failing.
