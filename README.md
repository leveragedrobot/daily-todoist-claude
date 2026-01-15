# Daily Todoist for Claude Code

A Claude Code custom command that reviews your Todoist tasks and categorizes them by what AI can handle autonomously vs. what requires human action.

## What It Does

When you run `/daily-todoist`, Claude will:

1. **Fetch** all your tasks due today, overdue items, and inbox
2. **Categorize** each task into:
   - **Claude CAN Complete** - Research, writing, code, file operations
   - **Need Approval** - Tasks Claude can do but should confirm first
   - **Human Required** - Physical tasks, calls, personal decisions
3. **Present** a summary table with quick stats
4. **Execute** any tasks you approve, marking them complete in Todoist

## Prerequisites

- [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code) installed
- [Todoist MCP Server](https://github.com/anthropics/claude-mcp-server-todoist) configured

## Installation

### Option 1: Clone and Symlink (Recommended)

```bash
# Clone the repo
git clone https://github.com/leveragedrobot/daily-todoist-claude.git ~/daily-todoist-claude

# Create the plugins directory if it doesn't exist
mkdir -p ~/.claude/plugins/marketplaces/claude-plugins-official/plugins

# Symlink to Claude's plugin directory
ln -s ~/daily-todoist-claude ~/.claude/plugins/marketplaces/claude-plugins-official/plugins/daily-todoist-claude
```

### Option 2: Direct Clone

```bash
git clone https://github.com/leveragedrobot/daily-todoist-claude.git \
  ~/.claude/plugins/marketplaces/claude-plugins-official/plugins/daily-todoist-claude
```

## Usage

In any Claude Code session:

```
/daily-todoist
```

Claude will analyze your tasks and present a categorized summary. You can then tell Claude which tasks to complete (by number or "all").

## Example Output

```
## Todoist Daily Review - January 14, 2026

### Ready to Complete (Claude can do now)
| # | Task | Project | Why Claude Can Do It |
|---|------|---------|---------------------|
| 1 | Research competitor pricing | Work | Web research task |
| 2 | Draft meeting agenda | Work | Writing/planning task |
| 3 | Create backup script | Personal | Code task |

### Human Required
| Task | Project | Why |
|------|---------|-----|
| Call dentist | Personal | Phone call required |
| Buy groceries | Personal | Physical task |

### Quick Stats
- Total tasks reviewed: 12
- Claude can handle: 3 (25%)
- Overdue items: 2
```

## Customization

Edit `commands/daily-todoist.md` to:
- Add new task categories Claude can handle
- Adjust the categorization logic
- Change the output format

## License

MIT
