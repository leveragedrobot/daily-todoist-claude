---
name: daily-todoist
description: Review Todoist tasks and identify which ones Claude can complete autonomously
version: 1.0.0
---

# Daily Todoist Review

Scans all Todoist tasks and categorizes them by what Claude can handle vs. what requires human action.

## Step 1: Fetch All Tasks

Use the Todoist MCP tools to get a comprehensive view:

1. First get the overview of all projects:
```
Use mcp__todoist__get-overview (no projectId) to see all projects
```

2. Then fetch tasks due today and overdue:
```
Use mcp__todoist__find-tasks-by-date with startDate="today", limit=100
```

3. Also fetch tasks from inbox (often where captured items land):
```
Use mcp__todoist__find-tasks with projectId="inbox", limit=50
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
| Task | Project | Why Claude Can Do It |
|------|---------|---------------------|
| ... | ... | ... |

**Want me to complete any of these?** Reply with the task numbers or "all".

### Need Your Approval First
| Task | Project | What Claude Would Do |
|------|---------|---------------------|
| ... | ... | ... |

### Human Required
| Task | Project | Why |
|------|---------|-----|
| ... | ... | ... |

### Quick Stats
- Total tasks reviewed: X
- Claude can handle: X (Y%)
- Overdue items: X
```

## Step 4: Execute Approved Tasks

When the user approves tasks:
1. Work through them one at a time
2. Mark each as complete in Todoist using `mcp__todoist__complete-tasks`
3. If a task spawns subtasks, add them to Todoist
4. Report results for each completed task

## Notes

- Be conservative - if unsure whether Claude can complete a task, put it in "Need Approval"
- For vague tasks like "Handle X", ask for clarification before attempting
- If a task is blocked by something, note the blocker
- Consider task priorities (p1 = urgent, p4 = low)
