---
name: sqlew-decision-format
description: |
  REQUIRED format for decisions and constraints in plan documents.
  Use this skill when drafting plans that must be converted into sqlew decisions/constraints with low variance.
  Enforce 📌 Decision / 🚫 Constraint markers and canonical field names.
---

## Use This Format

When plan text contains architecture decisions or constraints, write them in the exact blocks below.

### 📌 Decision: [hierarchical/key]

```markdown
### 📌 Decision: [key/path]
- **Value**: Description of the decision
- **Layer**: presentation | business | data | infrastructure | cross-cutting
- **Tags**: tag1, tag2 (optional)
- **Rationale**: Why this decision was made (optional)
- **Alternatives**: Option A, Option B (optional, comma-separated)
- **Tradeoffs**: Pros and cons description (optional)
```

Rules:
- Keep `key/path` stable across revisions.
- Prefer one decision per block.
- If `Rationale` exists, include `Alternatives` and `Tradeoffs` when possible.

Example:

### 📌 Decision: frontend/framework
- **Value**: Use Astro for static-first portfolio
- **Layer**: presentation
- **Tags**: astro, frontend, ssg
- **Rationale**: Static hosting on Xserver requires low operational complexity
- **Alternatives**: Next.js static export, Eleventy
- **Tradeoffs**: Faster and simpler deploy, but fewer built-in dynamic runtime capabilities

### 🚫 Constraint: [category]

```markdown
### 🚫 Constraint: [category]
- **Rule**: Description of the constraint
- **Priority**: critical | high | medium | low
- **Tags**: tag1, tag2 (optional)
```

Category options:
- `architecture`
- `security`
- `code-style`
- `performance`

Example:

### 🚫 Constraint: security
- **Rule**: No hardcoded secrets. Use environment variables only.
- **Priority**: critical
- **Tags**: security, secrets

## Codex Operation Note

Claude Code Hooks are not available in Codex. Use this manual conversion flow after writing plan blocks:

1. Parse each `📌 Decision` block.
2. Register decision with `mcp__sqlew__decision(action="set", ...)`.
3. If `Rationale` exists, add context with `mcp__sqlew__decision(action="add_decision_context", ...)`.
4. Parse each `🚫 Constraint` block.
5. Register constraint with `mcp__sqlew__constraint(action="set", ...)`.

## Mapping to sqlew Fields

- `Decision key` -> `decision.key`
- `Value` -> `decision.value`
- `Layer` -> `decision.layer`
- `Tags` -> `decision.tags`
- `Rationale` -> `decision_context.rationale`
- `Alternatives` -> `decision_context.alternatives_considered`
- `Tradeoffs` -> `decision_context.tradeoffs`
- `Constraint category` -> `constraint.category`
- `Rule` -> `constraint.text`
- `Priority` -> `constraint.priority`
