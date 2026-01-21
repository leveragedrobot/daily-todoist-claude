---
name: daily-todoist
description: Review Todoist tasks and identify which ones Claude can complete autonomously
version: 2.0.0
---

# Daily Todoist Review

Scans all Todoist tasks and categorizes them by what Claude can handle vs. what requires human action.

## Step 1: Fetch All Tasks

Use the `td` CLI (Todoist CLI) with JSON output:

```bash
# Get today's tasks + overdue
td today --json

# Get inbox tasks (captured items)
td inbox --json

# Get upcoming tasks (next 7 days)
td upcoming 7 --json
```

## Step 2: Categorize Tasks

Review each task and categorize into these buckets:

### Claude CAN Complete (Autonomously)
Tasks Claude can do right now without human involvement:
- **Research**: "Look up...", "Find...", "Research...", "Compare..."
- **Writing/Drafting**: "Write...", "Draft...", "Create outline...", "Summarize..."
- **Code/Scripts**: "Create script to...", "Fix bug in...", "Update config..."
- **File Operations**: "Organize...", "Clean up...", "Move files..."
- **Data Tasks**: "Update spreadsheet...", "Extract data from..."
- **Planning**: "Create plan for...", "Make checklist...", "Outline steps..."

### Claude CAN Help (With Approval)
Tasks Claude can do but should confirm first:
- Sending emails or messages
- Making purchases or bookings
- Deleting or archiving files
- Any task involving external services
- Tasks with financial implications

### Human Required
Tasks only you can do:
- Physical tasks ("Buy groceries", "Call dentist")
- Tasks requiring human judgment or relationships
- Sensitive personal decisions
- Tasks requiring physical presence
- Anything with "waiting for" someone else

## Step 3: Present Summary

Format the output as:

```
## Todoist Daily Review - [Today's Date]

### Ready to Complete (Claude can do now)
| # | Task | ID | Priority | Project | Why Claude Can Do It |
|---|------|-----|----------|---------|---------------------|
| 1 | ... | 123 | P2 | ... | ... |

**Want me to complete any of these?** Reply with the task numbers or "all".

### Need Your Approval First
| # | Task | ID | Priority | Project | What Claude Would Do |
|---|------|-----|----------|---------|---------------------|
| 1 | ... | 456 | P1 | ... | ... |

### Human Required
| Task | Priority | Project | Why |
|------|----------|---------|-----|
| ... | P3 | ... | ... |

### Quick Stats
- Total tasks reviewed: X
- Claude can handle: X (Y%)
- Overdue items: X
- High priority (P1/P2): X
```

## Step 4: Execute Approved Tasks

When the user approves tasks:
1. Work through them one at a time
2. Mark each as complete in Todoist:
   ```bash
   td task complete <task-id>
   ```
3. If a task spawns subtasks, add them:
   ```bash
   td add "New task description"
   ```
4. Add comments to tasks if needed:
   ```bash
   td comment add --task-id <task-id> "Comment text"
   ```
5. Report results for each completed task

## Useful td CLI Commands

```bash
# List tasks
td today --json              # Today + overdue
td inbox --json              # Inbox
td upcoming 14 --json        # Next 14 days

# Manage tasks
td add "Task name"           # Quick add with natural language
td task complete <id>        # Mark complete
td task view <id>            # View task details

# Comments
td comment add --task-id <id> "Comment"
td comment list --task-id <id>

# Projects
td project list --json       # List all projects
```

## Notes

- Be conservative - if unsure whether Claude can complete a task, put it in "Need Approval"
- For vague tasks like "Handle X", ask for clarification before attempting
- If a task is blocked by something, note the blocker
- Consider task priorities (p1 = urgent, p4 = low) when presenting tasks
- Use `--json` flag for parseable output
- **Recurring tasks**: Completing a recurring task creates the next occurrence automatically
- **Error handling**: If `td` commands fail, check that the CLI is installed (`td --version`) and authenticated (`td sync`)
