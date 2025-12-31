# /plan Command

Create or list implementation plans.

## Usage

- `/plan` - List all existing plans with progress
- `/plan <description>` - Create a new plan for the given task

## When creating a plan:

1. **Explore the codebase** to understand existing patterns
2. **Break down the task** into specific, actionable todos
3. **Create the plan file** at `.claude/plans/{slug}_{hash}.plan.md`

## Plan File Format

```yaml
---
name: Plan Title
overview: Brief description
todos:
  - id: unique-id
    content: Task description
    status: pending
    dependencies:
      - other-id
---

# Plan Title

## Overview
[Detailed description]

## Implementation
[Step by step guide with file paths and code examples]
```

## File Naming

- Slug: lowercase, underscores (e.g., `add_authentication`)
- Hash: 8 random hex characters
- Example: `add_authentication_a1b2c3d4.plan.md`

## After Creating

Ask: "Would you like me to start executing this plan?"

If yes, use `/plan-execute` or invoke the plan-executor agent.

$ARGUMENTS
