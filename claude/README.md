# Claude Code PRD Workflow

A systematic approach to feature development using Claude Code with custom slash commands for Product Requirements Documents (PRDs), task lists, and implementation tracking.

## Overview

This repository includes three custom Claude Code commands that streamline the development process from idea to implementation:

- `/create-prd` - Generate detailed Product Requirements Documents
- `/create-tasklist` - Convert PRDs into structured task lists
- `/work-tasks` - Manage implementation one sub-task at a time

## Setup

### Prerequisites
- [Claude Code](https://claude.ai/code) installed
- Global commands configured in `~/.claude/commands/`

### Directory Structure
```
your-project/
├── tasks/                          # All PRD and task files
│   ├── prd-[feature-name].md      # Generated PRDs
│   └── [feature-name]/            # Feature directories
│       └── tasks-[feature-name].md # Task lists
├── CLAUDE.md                       # Project configuration
└── [your project files]
```

## Workflow

### 1. Create PRD (`/create-prd`)

Start Claude Code and describe your feature idea:

```bash
claude

/create-prd

I want to build a user authentication system with login, signup, and password reset.
```

**What happens:**
- Claude asks clarifying questions about requirements
- Generates a comprehensive PRD at `tasks/prd-[feature-name].md`
- Includes user stories, functional requirements, and acceptance criteria

### 2. Generate Task List (`/create-tasklist`)

Convert your PRD into actionable tasks:

```bash
/create-tasklist

Please read the PRD file at: tasks/prd-user-authentication.md
```

**What happens:**
- Claude shows high-level tasks for review
- Say "Go" to generate detailed sub-tasks
- Creates task list at `tasks/user-authentication/tasks-user-authentication.md`

### 3. Implement Features (`/work-tasks`)

Work through tasks systematically:

```bash
/work-tasks

Please work through the task list at: tasks/user-authentication/tasks-user-authentication.md
```

**What happens:**
- Claude works on one sub-task at a time
- Asks permission before starting each sub-task
- Updates task list with completion status `[x]`
- Stops after each task for your approval

## Key Commands

| Command | Purpose | Usage |
|---------|---------|-------|
| `/create-prd` | Generate PRD from feature description | Provide 1-2 sentence feature description |
| `/create-tasklist` | Convert PRD to task list | Provide full path to PRD file |
| `/work-tasks` | Implement tasks one by one | Provide full path to task list file |

## Best Practices

### Creating PRDs
- Keep initial description focused (1-2 sentences)
- Answer clarifying questions thoroughly
- Review and refine the generated PRD

### Task Generation
- Review high-level tasks before saying "Go"
- Ask for changes if tasks don't cover important aspects
- Ensure task breakdown makes sense for your project

### Implementation
- Use `/clear` for fresh sessions when resuming work
- Work one sub-task at a time - don't rush
- Ask questions if sub-tasks are unclear
- Review each completion before continuing

## Resuming Work

After using `/clear`, quickly restore context:

```bash
/work-tasks

Please continue working through the task list at: tasks/[feature-name]/tasks-[feature-name].md
```

Claude will automatically identify completed tasks and pick up where you left off.

## Example Complete Workflow

```bash
# 1. Create PRD
/create-prd
I want to build a todo list app with categories and due dates.

# 2. Generate tasks (after PRD is created)
/create-tasklist
Please read the PRD file at: tasks/prd-todo-list-app.md

# 3. Implement (after saying "Go" and tasks are generated)
/work-tasks
Please work through the task list at: tasks/todo-list-app/tasks-todo-list-app.md
```

## File Organization

- **PRDs**: `tasks/prd-[feature-name].md`
- **Task Lists**: `tasks/[feature-name]/tasks-[feature-name].md`
- **Code**: Follow structure suggested in task lists
- **Tests**: Place alongside source files (e.g., `Component.tsx` + `Component.test.tsx`)

## Tips

- Start with simple features to get familiar with the workflow
- Use descriptive feature names for better file organization
- Review PRDs with your team before generating task lists
- Commit task list updates to track progress over time

---

*This workflow emphasizes systematic development with clear documentation and step-by-step implementation, making it ideal for both solo developers and team collaboration.*
