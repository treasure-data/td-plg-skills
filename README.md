# td-plg-skills

Claude Code skill marketplace for the Treasure AI Preview. This Repo is Public.

## Installing

```
/plugin marketplace add treasure-data/td-plg-skills
```

## Plugin: treasure-ai-plg

Guided-flow router and goal playbooks for the Treasure AI Studio PLG playground.

### Skills

- **`parse`** — guided-flow controller. Runs first on every user message, classifies intent (Strong / Medium / None), and dispatches to the matching goal playbook.

### Goals

- **analyze-data** — explore and analyze customer data
- **campaign-planning** — plan a CDP marketing campaign
- **find-segment** — find and define a customer segment
- **journey-planning** — plan a CDP customer journey
- **learn-about-treasure-ai** — answer questions about Treasure AI
- **general-questions** — handle off-topic questions with a nudge back

### Routing model

| Intent | Trigger | Action |
|--------|---------|--------|
| Strong | User asks for a concrete artifact | Dispatch directly into the matching goal |
| Medium | User asks conceptually about a goal-relevant topic | Brief answer + AskUserQuestion offering the playbook |
| None | Off-topic / generic | Answer + soft inline nudge |

## Repository structure

```
.claude-plugin/
└── marketplace.json
skills/
└── parse/
    ├── SKILL.md
    ├── topic-map.md
    └── goals/
        ├── analyze-data.md
        ├── campaign-planning.md
        ├── find-segment.md
        ├── general-questions.md
        ├── journey-planning.md
        └── learn-about-treasure-ai.md
```
