# td-plg-skills

Skills for the Treasure AI Preview. This Repo is Public. 

## Skills

- **`plg-router/`** — guided-flow controller. Runs first on every user message, classifies intent (Strong / Medium / None), and dispatches to the matching playbook.
- **`onboarding/`** — first-run welcome and goal-picker flow. Invoked by `plg-router` when a first-run user has no clear intent, or when the user types `/onboarding`.

## Shared content

- **`goals/`** — playbook bodies (analyze-data, find-segment, journey-planning, campaign-planning, generate-report, explore). Read by both `plg-router` (on Strong/Medium dispatch) and `onboarding` (on first-run goal picker).
- **`shared/`** — shared question templates (`ask-industry.md`, `ask-data-source.md`) and the setup-complete breadcrumb (`breadcrumb.md`).

## How the pieces fit

```
plg-router  ─────dispatches to────→  goals/*.md
    │                                    ↑
    └── falls back to ──→ onboarding ────┘
                              │
                              └── reads shared/*.md
```

## Installing into a project

Drop the four folders into your project's `.claude/skills/`:

```
.claude/skills/
├── plg-router/
├── onboarding/
├── goals/
└── shared/
```

`goals/` and `shared/` are not skills themselves (no `SKILL.md`) — they're shared content folders that the two skills read into via relative paths (`../goals/...`, `../shared/...`).

## Routing model

| Intent | Trigger | Action |
|--------|---------|--------|
| Strong | User asks for a concrete artifact ("build me a re-engagement journey for VIPs") | Dispatch directly into the matching goal |
| Medium | User asks conceptually about a goal-relevant topic ("what's the difference between a journey and a workflow?") | Brief answer + AskUserQuestion offering the playbook |
| None | Off-topic / generic | Answer + soft inline nudge to onboarding |

The router uses **semantic matching** against goal descriptions in `plg-router/topic-map.md` — keywords are anchors, not a closed list, so unseen phrasings (`whales`, `wake up dormant users`, `revive my list`) route correctly.
