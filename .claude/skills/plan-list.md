---
name: plan-list
description: List all implementation plans and their progress. Usage: `/plan-list` or `/plans`
---

# Plan List Skill

Display all plans in `.claude/plans/` with their completion status.

## Instructions

1. **Find all plans** - Glob for `*.plan.md` files in `.claude/plans/`
2. **Parse each plan** - Extract YAML frontmatter
3. **Calculate progress** - Count completed vs total todos
4. **Display summary** - Show name, overview, and progress bar

## Output Format

```
# Implementation Plans

## Active Plans

1. **Image Resolution Resizing** (6/8 completed)
   Add image resizing functionality to pixo across all layers
   File: image_resolution_resizing_7ba1149e.plan.md
   [======----] 75%

2. **Improved PNG Quantization** (8/9 completed)
   Enhance pixo's default palette quantization
   File: improved_png_quantization_7b801a13.plan.md
   [=========-] 89%

## Completed Plans

3. **Rust Image Compression Library** (14/14 completed)
   Build a minimal-dependency Rust library
   File: rust_image_compression_library_3adbb238.plan.md
   [==========] 100%

---
Total: 3 plans, 28/31 todos completed (90%)
```

## Progress Calculation

```python
completed = sum(1 for t in todos if t['status'] == 'completed')
total = len(todos)
percentage = (completed / total) * 100
bar_filled = int(percentage / 10)
bar_empty = 10 - bar_filled
progress_bar = '[' + '=' * bar_filled + '-' * bar_empty + ']'
```

## Sorting

- Active plans (incomplete) first, sorted by % complete descending
- Completed plans last, sorted by name

## Quick Actions

After listing, suggest:
- `/plan-execute <name>` to continue work on a plan
- `/plan <task>` to create a new plan
- `/plan-update <name> <id> <status>` to manually update status
