---
name: plan-update
description: Update todo status in a plan file. Usage: `/plan-update <plan-name> <todo-id> <status>` or `/plan-update <plan-name>` to mark current in_progress task as completed.
---

# Plan Update Skill

Update the status of todos in plan files stored in `.claude/plans/`.

## Usage

```
/plan-update <plan-file-or-name> <todo-id> <status>
/plan-update <plan-file-or-name>  # Complete current in_progress task
```

## Instructions

### When updating a specific todo:

1. **Find the plan file** in `.claude/plans/` matching the provided name or filename
2. **Read the plan file** to get current YAML frontmatter
3. **Update the todo status** for the matching `id`
4. **Write back** the updated file preserving all markdown content

### When no todo-id provided:

1. Find the first todo with `status: in_progress`
2. Mark it as `completed`
3. Find the next `pending` todo (respecting dependencies)
4. Mark it as `in_progress`
5. Report what was completed and what's now in progress

### Status values:
- `pending` - Not started
- `in_progress` - Currently being worked on
- `completed` - Done
- `cancelled` - No longer needed

### Dependency handling:

When marking a task as `in_progress`, verify all its dependencies are `completed`. If not, report the blocking dependencies.

## Implementation

Parse the YAML frontmatter between `---` markers:

```python
# Pseudo-code for parsing
content = read_file(plan_path)
parts = content.split('---', 2)  # ['', yaml_content, markdown_content]
yaml_data = parse_yaml(parts[1])
markdown = parts[2]

# Update the specific todo
for todo in yaml_data['todos']:
    if todo['id'] == target_id:
        todo['status'] = new_status
        break

# Write back
new_content = f"---\n{dump_yaml(yaml_data)}---\n{markdown}"
write_file(plan_path, new_content)
```

## Output Format

After updating, report:

```
Updated plan: {plan_name}
- {todo_id}: {old_status} -> {new_status}

Progress: {completed}/{total} todos completed
Next up: {next_pending_todo_content}
```

## Example

```
> /plan-update image_resolution resize-module completed

Updated plan: Image Resolution Resizing
- resize-module: pending -> completed

Progress: 1/8 todos completed
Next up: Implement bilinear interpolation in src/resize.rs
```
