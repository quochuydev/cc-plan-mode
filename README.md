# claude-plan-mode

Plan-based development workflow for Claude Code. Create, track, and execute implementation plans with YAML frontmatter todos and progress tracking.

## Features

- **Structured Plans** - YAML frontmatter with todos, dependencies, and status tracking
- **Progress Tracking** - Visual progress bars and completion percentages
- **Dependency Resolution** - Tasks respect dependencies before starting
- **Slash Commands** - `/plan`, `/plan-execute`, `/plan-update`, `/plan-list`

## Installation

Copy the `.claude` folder to your project root:

```bash
cp -r .claude /path/to/your/project/
```

Or clone and copy:

```bash
git clone https://github.com/YOUR_USERNAME/claude-plan-mode.git
cp -r claude-plan-mode/.claude /path/to/your/project/
```

## Usage

### Create a Plan

```
/plan add user authentication with JWT
```

Creates `.claude/plans/user_authentication_a1b2c3d4.plan.md` with structured todos.

### List Plans

```
/plan
```

Shows all plans with progress:

```
# Implementation Plans

1. **User Authentication** (2/5 completed)
   Add JWT-based auth with login/logout
   [==--------] 40%

2. **API Refactor** (8/8 completed)
   Restructure API endpoints
   [==========] 100%
```

### Execute Next Task

```
/plan-execute user_authentication
```

Finds the next pending task, executes it, and updates the plan file.

### Update Status Manually

```
/plan-update user_authentication auth-middleware completed
```

Or complete current task and start next:

```
/plan-update user_authentication
```

## Plan Format

Plans use YAML frontmatter with markdown body:

```yaml
---
name: User Authentication
overview: Implement JWT-based authentication with login/logout endpoints.
todos:
  - id: auth-middleware
    content: Create authentication middleware
    status: pending
  - id: jwt-utils
    content: Implement JWT sign/verify utilities
    status: pending
    dependencies:
      - auth-middleware
  - id: login-endpoint
    content: Add POST /api/auth/login endpoint
    status: pending
    dependencies:
      - jwt-utils
---

# User Authentication

## Overview
Implement JWT-based authentication...

## Implementation Details
...
```

### Todo Statuses

| Status | Description |
|--------|-------------|
| `pending` | Not started |
| `in_progress` | Currently working on |
| `completed` | Done |
| `cancelled` | No longer needed |

### Dependencies

Tasks with `dependencies` won't start until all listed task IDs are `completed`.

## File Structure

```
.claude/
├── commands/
│   ├── plan.md           # /plan command
│   ├── plan-execute.md   # /plan-execute command
│   └── plan-update.md    # /plan-update command
├── plans/                # Your plan files go here
│   └── *.plan.md
├── settings.yaml         # Configuration
└── skills/
    ├── plan.md           # Plan creation logic
    ├── plan-execute.md   # Execution logic
    ├── plan-list.md      # List/progress logic
    └── plan-update.md    # Status update logic
```

## Configuration

Edit `.claude/settings.yaml`:

```yaml
context:
  plans_directory: .claude/plans
```

## Tips

- Use short, descriptive `id` values (e.g., `auth-middleware`, `setup-db`)
- Add `dependencies` to enforce task order
- Include file paths and code examples in the plan markdown body
- Run `/plan-execute <name> --all` to execute all remaining tasks sequentially

## License

MIT
