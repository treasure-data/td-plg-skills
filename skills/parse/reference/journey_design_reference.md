#### Technical reference for journey design

**Journey-level structure**:
```yaml
type: journey           # optional
name: Journey Name
description: ...
reentry: no_reentry | reentry_unless_goal_achieved | reentry_always
goal:                   # CriteriaDef object — NOT a bare segment key
  name: Goal Name
  segment: goal_segment_key
segments: { ... }
activations: { ... }
stages: [ ... ]         # flat format
# OR versioned format:
journeys:
  - state: draft
    goal: { name: ..., segment: ... }
    stages: [ ... ]
```

**CriteriaDef format** (shared by `goal`, `entry_criteria`, `milestone`, `exit_criteria`):
```yaml
name: Human-readable name    # required
segment: segment_key         # required — key from segments: section
target:                      # optional — jump to another journey
  journey: Other Journey
  stage: Stage Name
```

**Stage fields**:
```yaml
stages:
  - name: "Stage 1: Welcome"
    entry_criteria:       # CriteriaDef — who enters this stage
      name: New Signups
      segment: new_signups
    milestone:            # CriteriaDef — who graduates to next stage
      name: Email Engaged
      segment: email_engaged
    exit_criteria:        # CriteriaDef[] — who gets removed (churned)
      - name: Churned Users
        segment: churned_users
    steps: [ ... ]
```
Key rule: Stage 2+ `entry_criteria` typically reuses the previous stage's `milestone` segment.

**Step types**: `activation`, `wait`, `decision_point`, `ab_test`, `merge`, `end`, `jump`

**Step format** — every step has a `name` (display label) and `uuid` (identifier for `next`/branch references), and nests type-specific properties under `with`:
```yaml
steps:
  # Activation — sends a message or triggers an action
  - uuid: send_welcome_email
    type: activation
    name: Send Welcome Email
    with:
      activation: welcome_email_activation
    next: wait_for_open

  # Wait — pause for a fixed duration
  - uuid: wait_for_open
    type: wait
    name: Wait 3 Days
    with:
      duration: 3
      unit: day
    next: check_opened

  # Wait — pause until a condition is met (no duration)
  - uuid: wait_for_purchase
    type: wait
    name: Wait for Purchase
    with:
      condition: purchased_segment

  # Decision point — branch on segment membership
  - uuid: check_opened
    type: decision_point
    name: Email Open Check
    with:
      branches:
        - name: opened
          segment: email_opened_segment
          next: send_followup
        - name: not_opened
          excluded: true
          next: send_reminder

  # A/B test — split traffic by percentage
  - uuid: test_subject_line
    type: ab_test
    name: Subject Line Test
    with:
      variants:
        - name: variant_a
          percentage: 50
          next: send_version_a
        - name: variant_b
          percentage: 50
          next: send_version_b

  # Merge — converge multiple branches
  - uuid: post_branch_merge
    type: merge
    name: Merge Branches
    next: wait_after_merge

  # End — terminal step (no next, no with)
  - uuid: journey_end
    type: end
    name: Journey End

  # Jump — jump to a step in another stage
  - uuid: jump_to_nurture
    type: jump
    name: Jump to Nurture
    next: nurture_stage_entry
```

Key rules:
- Use `uuid` (not `id` or `name`) as the step identifier — this is the value referenced by `next` and branch targets.
- `next` goes on the step itself (not inside `with`).
- `end` steps have no `next` and no `with`.
- The first step in the array is the root step (entry point) unless `root_step` is set on the stage.

**Segment rule format**:
```yaml
segment_name:
  description: Human-readable description
  rule:
    type: And
    conditions:
      - type: Value
        attribute: attribute_name
        operator:
          type: Equal | GreaterEqual | LessEqual | TimeWithinPast | ...
          value: "value"
```

**Activation format**:
```yaml
activation_name:
  name: Human-readable name
  connection: Connection Name
  all_columns: true
  run_after_journey_refresh: true
  connector_config:
    de_name: DataExtensionName
    data_operation: upsert
```

**Flow control rules**:
- Every branch must perform a different action (otherwise don't branch)
- Multiple paths must converge through a Merge step (not directly to End)
- A wait step must follow every activation step (via merge is OK)
- End step has no `next` and no `with`
- Decision point = fixed delay then check; condition wait = react immediately with timeout
- The "excluded" branch is the catch-all and needs no segment
- Condition wait paths must lead to different actions (otherwise use duration wait)
