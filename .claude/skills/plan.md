---
name: plan
description: Create or view implementation plans. Use `/plan <task>` to create a new plan or `/plan` to list existing plans.
---

# Plan Mode Skill

Create structured implementation plans with YAML frontmatter, todos tracking, and detailed documentation.

## Plan Storage

Plans are stored in `.claude/plans/` directory. Each plan file uses the format: `{slug}_{hash}.plan.md`

## Plan Format

Plans follow this structure:

```yaml
---
name: Plan Title
overview: Brief description of the plan objective
todos:
  - id: unique-task-id
    content: Task description
    status: pending|in_progress|completed|cancelled
    dependencies:
      - other-task-id
---

# Plan Title

## Overview
Detailed description...

## Implementation Details
...
```

## Instructions

### When creating a new plan:

1. **Analyze the task** - Understand what needs to be done
2. **Explore the codebase** - Use Glob, Grep, Read to understand existing patterns
3. **Break down into todos** - Create specific, actionable tasks with unique IDs
4. **Document the approach** - Write detailed implementation notes
5. **Save the plan** - Write to `.claude/plans/{slug}_{8char_hash}.plan.md`

### Plan file naming:
- Convert plan name to lowercase slug (spaces to underscores)
- Append 8-character random hex hash
- Example: `user_authentication_a1b2c3d4.plan.md`

### Todo statuses:
- `pending` - Not started
- `in_progress` - Currently being worked on
- `completed` - Done
- `cancelled` - No longer needed

### When listing plans:
- Read all `.plan.md` files from `.claude/plans/`
- Show name, overview, and completion status (completed/total todos)

## Example Output

When creating a plan, produce output like:

```markdown
---
name: Add User Authentication
overview: Implement JWT-based authentication with login/logout endpoints and protected routes middleware.
todos:
  - id: auth-middleware
    content: Create authentication middleware in src/middleware/auth.ts
    status: pending
  - id: jwt-utils
    content: Implement JWT sign/verify utilities in src/lib/jwt.ts
    status: pending
    dependencies:
      - auth-middleware
  - id: login-endpoint
    content: Add POST /api/auth/login endpoint
    status: pending
    dependencies:
      - jwt-utils
  - id: logout-endpoint
    content: Add POST /api/auth/logout endpoint
    status: pending
    dependencies:
      - jwt-utils
  - id: protected-routes
    content: Apply auth middleware to protected API routes
    status: pending
    dependencies:
      - auth-middleware
      - login-endpoint
---

# Add User Authentication

## Overview

Implement JWT-based authentication system...

## Architecture

[Include mermaid diagrams if helpful]

## Implementation Steps

### 1. Authentication Middleware
...
```

## Workflow

1. If user provides a task description, create a new plan
2. If no task provided, list existing plans with their status
3. After creating a plan, ask if user wants to start executing it

Remember: Plans should be thorough but focused. Include file paths, code examples, and clear dependencies between tasks.
