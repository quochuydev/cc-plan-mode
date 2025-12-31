# /plan-execute Command

Execute tasks from a plan file, updating status as you go.

## Usage

- `/plan-execute <plan-name>` - Execute the next pending task
- `/plan-execute <plan-name> --all` - Execute all remaining tasks

## Execution Steps

1. **Read the plan** from `.claude/plans/`
2. **Find the next task**:
   - First check for `status: in_progress`
   - If none, find first `pending` with all dependencies `completed`
3. **Execute the task** following the plan's implementation details
4. **Update the plan file**:
   - Mark completed task as `completed`
   - Mark next task as `in_progress`
5. **Report progress**

## Updating Plan Status

Use the Edit tool to modify the YAML frontmatter:

```yaml
# Before
  - id: task-id
    status: in_progress

# After
  - id: task-id
    status: completed
```

## Dependency Check

Before starting a task, verify all its dependencies are completed:

```yaml
dependencies:
  - dep-1  # Must be completed
  - dep-2  # Must be completed
```

If dependencies are not met, report which tasks must be completed first.

## Progress Report

After each task:
```
Completed: {task_id} - {task_content}
Progress: {completed}/{total} ({percentage}%)
Next: {next_task_content}
```

$ARGUMENTS
