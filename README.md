# td-plg-skills

Claude Code skill marketplace for the Treasure AI Preview. This Repo is Public.

## Installing

```
/plugin marketplace add treasure-data/td-plg-skills
```

## Plugin: treasure-ai-plg

PLG router and onboarding skills for Treasure AI Studio.

### Skills

- **`plg-router`** — guided-flow controller. Runs first on every user message, classifies intent (Strong / Medium / None), and dispatches to the matching playbook.
- **`onboarding`** — first-run welcome and goal-picker flow. Invoked by `plg-router` when a first-run user has no clear intent, or when the user types `/onboarding`.

### Shared content

- **`goals/`** — playbook bodies (analyze-data, find-segment, journey-planning, campaign-planning, generate-report, explore).
- **`shared/`** — shared question templates and the setup-complete breadcrumb.

### Routing model

| Intent | Trigger | Action |
|--------|---------|--------|
| Strong | User asks for a concrete artifact | Dispatch directly into the matching goal |
| Medium | User asks conceptually about a goal-relevant topic | Brief answer + AskUserQuestion offering the playbook |
| None | Off-topic / generic | Answer + soft inline nudge to onboarding |

## Repository structure

```
.claude-plugin/
└── marketplace.json
plugins/
└── treasure-ai-plg/
    ├── .claude-plugin/
    │   └── plugin.json
    └── skills/
        ├── plg-router/
        ├── onboarding/
        ├── goals/
        └── shared/
```
