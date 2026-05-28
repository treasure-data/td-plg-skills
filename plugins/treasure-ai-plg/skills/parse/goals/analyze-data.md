# Goal: Analyze Data

## Workflow

1. **Greet the user** — acknowledge their goal ("analyze data") and output preference. Be concise and action-oriented.

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

3. **Explore data** — briefly explain what data is available and why it matters before showing tables. Based on the data source answer:
   - Synthetic data: list available tables in the database and describe their schemas (table names, key columns). Skip row counts, column distributions, and data quality checks — defer those to the analysis execution step.
   - Upload my own data: wait for file upload, then inspect the file structure (columns, types). Skip row counts.

4. **Ask analysis purpose** — explain why knowing the goal matters (the same data can answer very different questions depending on intent). Use AskUserQuestion with:
   - Question: "What would you like to learn from your data?"
   - Single-select, options:
     | Label | Description |
     |-------|-------------|
     | Understand my customers | Who they are, what they do, and why |
     | Track performance | Measure KPIs, trends, or campaign results |
     | Find opportunities | Discover growth areas, segments, or patterns |

   If the user selects "Other", ask a free-text follow-up using AskUserQuestion.

   Use the selected purpose to guide step 5 (analysis approach suggestions) and step 7 (result framing).

5. **Suggest analysis approaches** — explain what an analysis approach is and why you're recommending these specific ones. Based on the user's analysis purpose and industry, offer 2–3 concrete analysis ideas. Use AskUserQuestion to let them pick or describe their own. Examples by industry:
   - Retail: basket analysis, customer lifetime value, seasonal trends
   - CPG: brand switching, promotion lift, channel mix
   - Travel: booking funnel, destination affinity, loyalty tier analysis
   - Automotive: lead-to-sale conversion, service retention, model preference
   - Media: content engagement, churn prediction, audience overlap
   - D2C: cohort retention, acquisition channel ROI, repeat purchase rate
   - B2B Tech: pipeline velocity, product adoption, expansion revenue

6. **Execute analysis** — before running analysis queries, check row counts for the tables being used to inform result assessment. Then run queries, compute metrics, and build the output.

7. **Deliver results** — present the analysis results as a written summary with key takeaways. Include a data table when the analysis benefits from showing specific numbers. If the task produces a deliverable file, call `mcp__tas__open_file` to display it.

## Behavior

- **Explain before asking** — before each question or decision point, briefly explain the concept and why it matters. Assume the user is new to data analysis. Keep explanations concise (1–2 sentences).
- Ask one question at a time. Do not overwhelm the user.
- Proactively suggest next steps after each deliverable.
- If the user's request doesn't make sense with the available data, suggest alternatives rather than failing.
