#### Technical reference for journey design

**Step types**: `activation`, `wait`, `decision_point`, `ab_test`, `merge`, `end`, `jump`

**Step format** — every step uses `uuid` as its identifier and nests type-specific properties under `with`:
```yaml
steps:
  # Activation — sends a message or triggers an action
  - uuid: send_welcome_email
    type: activation
    with:
      activation: welcome_email_activation
    next: wait_for_open

  # Wait — pause for a fixed duration
  - uuid: wait_for_open
    type: wait
    with:
      duration: 3
      unit: day
    next: check_opened

  # Wait — pause until a condition is met (no duration)
  - uuid: wait_for_purchase
    type: wait
    with:
      condition: purchased_segment

  # Decision point — branch on segment membership
  - uuid: check_opened
    type: decision_point
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
    next: wait_after_merge

  # End — terminal step (no next, no with)
  - uuid: journey_end
    type: end

  # Jump — jump to a step in another stage
  - uuid: jump_to_nurture
    type: jump
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
