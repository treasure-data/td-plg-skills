# Setup-complete breadcrumb

When a flow finishes (final artifact delivered, or the user is fully set up), the assistant's final message must end with this exact sentence on its own paragraph:

> *That's the setup. From now on, I'll tailor responses to your industry and goals.*

## Why it must be exact

The `plg-router` skill scans recent assistant messages for the substring:

```
I'll tailor responses to your industry and goals
```

If found, the router treats the user as **already set up** and skips first-run onboarding nudges.

## Rules

- Emit verbatim — including the apostrophe in `I'll`, the exact phrase `tailor responses to your industry and goals`, and the closing period.
- Emit only on the message that **concludes** a flow (onboarding completion, goal completion). Never on intermediate steps.
- Do not paraphrase, abbreviate, translate, or rewrite.

Any flow that completes a goal — whether reached via `onboarding` or invoked directly by `plg-router` on strong intent — should emit this sentence so the router's state detection stays consistent.
