---
name: sqlew-pr-adr
description: |
  REQUIRED for pull request creation. Use this skill when drafting a PR body,
  preparing to run `gh pr create`, or reviewing PR description quality.
  Enrich the PR body with Architecture Decision Records (ADR) from sqlew so reviewers
  understand why changes were made, not only what changed.
---

## Required Workflow

When creating a pull request, include decision context from sqlew.
Group the PR description by architecture decision, not by file list only.

## Step 1: Inspect Diff

```bash
git diff <base-branch>...HEAD --stat
git diff <base-branch>...HEAD
```

Extract keywords from the diff:
- File paths, directories, module names
- Function, class, struct, or type names
- New imports, package names, or dependency additions

## Step 2: Reverse-Lookup Decisions

Search related decisions for each keyword.

Use sqlew suggest:

```text
mcp__sqlew__suggest(action="by_context", key="<keyword>")
mcp__sqlew__suggest(action="by_context", key="<module-name>", tags=["<tag>"])
```

Then fetch full decision details:

```text
mcp__sqlew__decision(action="get", key="<matched-key>", include_context="true")
```

Selection rules:
- Include results with relevance score >= 35
- Deduplicate decisions across keyword searches
- Limit to top 5 decisions by score
- Exclude deprecated decisions

## Step 3: Build PR Body by Decision

Use this structure:

```markdown
## Summary

<!-- 1-3 bullets of overall change -->

## Architecture Decisions

### [decision/key] - [Decision Value]

- **Rationale**: Why this decision was made
- **Alternatives**: Other options considered
- **Tradeoffs**: Pros and cons of the selected approach
- **Changes**:
  - `path/to/file1` - what changed and why
  - `path/to/file2` - what changed and why

## Other Changes

- `path/to/file3` - changes not tied to ADR

## Test Plan

- [ ] Test step 1
- [ ] Test step 2
```

## Step 4: Create PR

```bash
gh pr create --title "<title>" --body "<body>"
```

If using a here-doc body, verify markdown headings and bullet formatting before submission.

## Edge Cases

- No decisions found: omit `Architecture Decisions` and use standard PR format
- More than 5 decisions: keep top 5 and add `See /sqlew for full decision history.`
- Missing decision context: include decision key and value, skip empty rationale fields
- Only refactor/chore changes: ADR section may be omitted
