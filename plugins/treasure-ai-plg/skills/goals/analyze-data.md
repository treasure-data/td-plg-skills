# Goal: Analyze Data

## Workflow

1. **Greet the user** — acknowledge their goal ("analyze data") and output preference. Be concise and action-oriented.

2. **Ask industry** — follow `shared/ask-industry.md`.

3. **Ask data source** — follow `shared/ask-data-source.md`.

4. **Explore data** — briefly explain what data is available and why it matters before showing tables. Based on the data source answer:
   - Synthetic data: list available tables in the `treasurebikes` database, describe their schemas, and suggest which tables are most relevant to the user's industry.
   - Upload my own data: wait for file upload, then inspect the data structure.

5. **Ask analysis purpose** — explain why knowing the goal matters (the same data can answer very different questions depending on intent). Use AskUserQuestion with:
   - Question: "What would you like to learn from your data?"
   - Single-select, options:
     | Label | Description |
     |-------|-------------|
     | Understand my customers | Who they are, what they do, and why |
     | Track performance | Measure KPIs, trends, or campaign results |
     | Find opportunities | Discover growth areas, segments, or patterns |
     | Something else | Describe your own analysis goal |

   If the user selects "Something else", ask a free-text follow-up using AskUserQuestion.

   Use the selected purpose to guide step 6 (analysis approach suggestions) and step 8 (result framing).

6. **Suggest analysis approaches** — explain what an analysis approach is and why you're recommending these specific ones. Based on the user's analysis purpose and industry, offer 2–3 concrete analysis ideas. Use AskUserQuestion to let them pick or describe their own. Examples by industry:
   - Retail: basket analysis, customer lifetime value, seasonal trends
   - CPG: brand switching, promotion lift, channel mix
   - Travel: booking funnel, destination affinity, loyalty tier analysis
   - Automotive: lead-to-sale conversion, service retention, model preference
   - Media: content engagement, churn prediction, audience overlap
   - D2C: cohort retention, acquisition channel ROI, repeat purchase rate
   - B2B Tech: pipeline velocity, product adoption, expansion revenue

7. **Execute analysis** — run queries, compute metrics, and build the output.

8. **Deliver results** — present in the user's preferred output format:
   - Interactive chart: generate an HTML page with charts, call `mcp__tas__open_file`
   - Data table: present structured table, offer CSV/XLSX download
   - Written summary: natural language analysis with key takeaways

## Behavior

- **Explain before asking** — before each question or decision point, briefly explain the concept and why it matters. Assume the user is new to data analysis. Keep explanations concise (1–2 sentences).
- Ask one question at a time. Do not overwhelm the user.
- Proactively suggest next steps after each deliverable.
- If the user's request doesn't make sense with the available data, suggest alternatives rather than failing.
