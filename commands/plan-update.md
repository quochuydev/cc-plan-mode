# /plan-update Command

Update todo status in a plan file.

## Usage

- `/plan-update <plan-name> <todo-id> <status>` - Set specific status
- `/plan-update <plan-name>` - Complete current in_progress task and start next

## Status Values

- `pending` - Not started
- `in_progress` - Currently working on
- `completed` - Done
- `cancelled` - No longer needed

## Examples

```
/plan-update image_resolution resize-module completed
/plan-update improved_png_quantization
```

## Workflow

When updating without specifying todo-id:
1. Find first `in_progress` todo
2. Mark it `completed`
3. Find next `pending` todo (respecting dependencies)
4. Mark it `in_progress`
5. Report the transition

## Output

```
Updated: improved_png_quantization_7b801a13.plan.md
- perceptual-distance: in_progress -> completed
- perceptual-splitting: pending -> in_progress

Progress: 2/9 (22%)
```

$ARGUMENTS
