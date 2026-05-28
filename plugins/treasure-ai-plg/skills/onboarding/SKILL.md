---
name: onboarding
description: "INTERNAL skill — do NOT auto-trigger on user messages. Only invoked by the plg-router skill when a first-run user has no clear intent, or when the user explicitly types /onboarding. Plg-router owns all top-level message classification; this skill handles the first-run goal-picker flow only."
allowed-tools:
  - Read
  - Glob
  - Grep
  - Bash
  - Write
  - Edit
  - AskUserQuestion
---

# First-Time User Onboarding

Activate when the user identifies as a new user with a goal and output format preference, OR when invoked explicitly via `/onboarding`.

## Step 1 — Parse preferences

Extract the user's **goal** and **output format** from their message. If either is missing, ask using AskUserQuestion.

## Step 2 — Load goal workflow

Find this skill's directory:

```
Glob("**/onboarding/SKILL.md")
```

The `goals/` and `shared/` folders are siblings of this skill (one level up from `onboarding/`). Read the matching goal file:

| Goal | File |
|------|------|
| Analyze data | `../goals/analyze-data.md` |
| Identify segments | `../goals/find-segment.md` |
| Map journeys | `../goals/journey-planning.md` |
| Plan email campaigns | `../goals/campaign-planning.md` |
| Generate a report | `../goals/generate-report.md` |
| Explore freely | `../goals/explore.md` |

Also Read `../shared/ask-industry.md`, `../shared/ask-data-source.md`, and `../shared/breadcrumb.md`.

## Step 3 — Execute

Follow the loaded goal workflow. Use the shared question templates when asking for industry and data source. Always use AskUserQuestion (not plain text) for structured questions.

When referring to yourself, use **Treasure AI** or **Treasure AI Studio** — not "Treasure Data" or "Treasure Data CDP".

## Step 4 — Signal completion

When the goal workflow finishes, follow the rules in `../shared/breadcrumb.md` — emit the setup-complete sentence verbatim as the final paragraph of your last message.
