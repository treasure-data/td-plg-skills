# Goal: Generate a Report

## Workflow

1. **Greet the user** — acknowledge their goal ("generate a report") and output preference. Be concise and action-oriented.

2. **Ask industry** — follow `shared/ask-industry.md`.

3. **Ask data source** — follow `shared/ask-data-source.md`.

4. **Ask report focus** — use AskUserQuestion with:
   - Question: "What should the report focus on?"
   - Single-select, options:
     | Label | Description |
     |-------|-------------|
     | Performance overview | Key metrics, trends, and KPIs for a given period |
     | Audience insights | Demographics, behavior patterns, and segment profiles |
     | Campaign analysis | Campaign performance, attribution, and ROI |
     | Product analytics | Usage patterns, feature adoption, and engagement |
     | Custom topic | Describe your own report focus |

5. **Determine scope** — ask one follow-up question about the time period or specific focus area (e.g., "Which time period?" or "Which campaign?"). Use AskUserQuestion.

6. **Query and analyze** — pull relevant data, compute metrics, identify trends and insights. Tailor KPIs to the user's industry:
   - Retail: revenue, AOV, conversion rate, return rate
   - CPG: sell-through rate, distribution, market share
   - Travel: booking rate, RevPAR, occupancy, ancillary revenue
   - Automotive: leads, test drives, close rate, service revenue
   - Media: DAU/MAU, engagement time, ad revenue, subscriber growth
   - D2C: CAC, LTV, repeat rate, NPS
   - B2B Tech: ARR, pipeline, win rate, net retention

7. **Generate report** — compile into a polished deliverable:
   - Interactive chart: HTML page with charts, tables, and narrative sections, call `mcp__tas__open_file`
   - Data table: structured data with computed metrics, offer CSV/XLSX download
   - Written summary: executive summary with key findings, trends, and recommendations

## Behavior

- Ask one question at a time. Do not overwhelm the user.
- Use the AskUserQuestion tool for all structured questions — do NOT ask as plain text.
- Include both high-level insights and supporting data in the report.
- Proactively suggest follow-up analyses or deeper dives after delivering the report.
