# Basic Setup Example

This example demonstrates a minimal AI Trackdown setup for a small development team starting their first AI-enhanced project using manual documentation workflows.

## Project Context

**Project**: Simple Todo Application  
**Team Size**: 3 developers  
**AI Usage**: Basic code assistance and documentation  
**Timeline**: 4 weeks  
**Token Budget**: 10,000 tokens/month

## Setup Steps

### 1. Setup Project Structure

```bash
cd todo-app
mkdir -p tasks/epics tasks/issues tasks/tasks docs
```

### 2. Create Configuration Template

Copy from `templates/config-examples.md` and create `TASKTRACK.md`:
```yaml
# Simple Todo App Configuration
version: 1
project:
  name: "Simple Todo App"
  id_prefix: "TODO"
  default_epic_budget: 5000

ai:
  token_tracking:
    enabled: true
    providers:
      claude:
        model: "claude-3.5-sonnet"
        cost_per_1k_input: 0.003
        cost_per_1k_output: 0.015
```

### 3. Create First Epic

Use the epic template from `templates/epic-template.md` to create `tasks/epics/EPIC-001-core-todo-functionality.md`:
```markdown
---
id: EPIC-001
type: epic
title: Core Todo Functionality
status: in-progress
owner: @team-lead
created: 2025-01-07T09:00:00Z
target_date: 2025-02-01T00:00:00Z
token_budget: 5000
labels: [core, mvp]
---

# Core Todo Functionality

## Overview
Build the essential features needed for a functional todo application.

## Success Metrics
- Users can create, read, update, delete todos
- Responsive design works on mobile and desktop
- Fast performance (< 100ms for basic operations)

## Issues
- [ ] ISSUE-001: Todo CRUD operations
- [ ] ISSUE-002: User interface design
- [ ] ISSUE-003: Data persistence

## Token Budget
- Planning: 1,000 tokens
- Implementation: 3,000 tokens  
- Review & Testing: 1,000 tokens
```

### 4. Break Down into Issues

Using the issue template from `templates/issue-template.md`, create individual issue files:
- `tasks/issues/ISSUE-001-todo-crud-operations.md`
- `tasks/issues/ISSUE-002-user-interface-design.md`
- `tasks/issues/ISSUE-003-data-persistence.md`

### 5. Create AI Context Documentation

Use `templates/llms-txt-examples.md` to create `/llms.txt`:
```txt
# Simple Todo App
# Generated: 2025-01-07T15:00:00Z

## Project Overview
Simple todo application built with React and Node.js
Team: 3 developers
Timeline: 4 weeks MVP

## Key Files
/tasks/epics/: Project epics and major features
/tasks/issues/: Development issues
/src/: Application source code

## Current Focus
EPIC-001: Core Todo Functionality (just started)
- ISSUE-001: Todo CRUD operations (todo)
- ISSUE-002: User interface design (todo)
- ISSUE-003: Data persistence (todo)

## Quick Reference
View open work: Check status fields in task files
Check progress: Review epic and issue completion status  
Track tokens: Update token usage in task frontmatter
```

## File Structure

After setup using templates, your project structure looks like:

```
todo-app/
├── tasks/
│   ├── epics/
│   │   └── EPIC-001-core-todo-functionality.md
│   ├── issues/
│   │   ├── ISSUE-001-todo-crud-operations.md
│   │   ├── ISSUE-002-user-interface-design.md
│   │   └── ISSUE-003-data-persistence.md
│   └── tasks/
├── docs/
│   └── llms.txt
├── src/
│   ├── components/
│   ├── pages/
│   └── utils/
└── TASKTRACK.md
```

## Daily Workflow

### Developer Day 1: Start CRUD Implementation

1. Check current work by reviewing task status in `tasks/issues/ISSUE-001-todo-crud-operations.md`
2. Update the file to change status from `todo` to `in-progress` and add assignee
3. Track AI assistance by updating token usage in the task frontmatter

### AI Agent Interaction

When Alice asks Claude for help:

```markdown
Context for AI Agent:
- Working on ISSUE-001: Todo CRUD operations
- React frontend, Node.js backend
- See /docs/llms.txt for full project context
- Budget: 3000 tokens for implementation
```

Claude gets instant project understanding from `/docs/llms.txt` and can provide targeted help.

### End of Day: Update Progress

1. Commit work with task reference:
```bash
git add .
git commit -m "feat(ISSUE-001): implement todo creation API

Token-Usage: claude=456
AI-Context: REST API design and implementation"
```

2. Manually update task status in `ISSUE-001-todo-crud-operations.md`
3. Review updated progress by checking task files

## Token Budget Tracking

### Weekly Review

Review token usage by checking task files and updating your budget tracking spreadsheet or document:

```
Token Usage Report (Week 1)
===========================
EPIC-001: 1,234 tokens (24.7% of budget)
├── Claude: 856 tokens (API design, code review)
├── Copilot: 378 tokens (code completion)

By Issue:
├── ISSUE-001: 456 tokens (API implementation)
├── ISSUE-002: 234 tokens (UI planning)
├── ISSUE-003: 544 tokens (database design)

Budget Status: On track ✅
Efficiency: Good (high-value AI interactions)
```

## Key Learnings

### What Works Well
- **Simple Setup**: Template-based initialization gets team productive quickly
- **AI Context**: `/docs/llms.txt` eliminates repeated explanations
- **Token Awareness**: Manual budget tracking prevents cost surprises
- **Git Integration**: Natural workflow with existing tools

### Best Practices Discovered
- Keep AI context blocks concise but specific
- Track token usage immediately after AI interactions in task files
- Use conventional commit messages with task references
- Regular budget reviews prevent overruns

### Team Feedback
> "The markdown files are so much easier to read than Jira tickets. And manually tracking AI costs helps us optimize our prompts." - Alice, Frontend Developer

> "Having tasks next to code in git is perfect. No more context switching between tools." - Bob, Backend Developer

> "Manual token tracking showed us that Claude is more cost-effective for architecture discussions than GPT-4." - Carol, Team Lead

## Next Steps

This basic setup provides the foundation for:
1. **Adding Integrations**: Manually sync with GitHub Issues or other tools
2. **Advanced Workflows**: Custom templates and documentation patterns
3. **Team Scaling**: Role-based file organization and workflows
4. **Analytics**: Manual token optimization and reporting

The simple todo app example shows how AI Trackdown transforms development workflow with minimal setup overhead while providing immediate AI cost visibility and optimization through manual documentation practices.