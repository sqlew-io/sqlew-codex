# Project Agent Policy

## Mandatory Planning and ADR Workflow

1. During plan design, the agent MUST call `sqlew.suggest` before finalizing any plan-level decisions or constraints.
2. During plan authoring, the agent MUST use the `sqlew-decision-format` skill to format all decisions and constraints.
3. Once a plan is approved, the agent MUST record the relevant decisions and constraints in sqlew before starting implementation.

## Enforcement

- These rules are mandatory and not optional.
- If any rule cannot be executed due to tool or environment limitations, the agent MUST stop and report the blocker before proceeding.
