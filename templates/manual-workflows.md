# ai-trackdown Manual Workflow Guide

This document outlines manual processes for creating, updating, and managing tasks using the ai-trackdown markdown-based system without automation tools.

---

## Getting Started

### Initial Project Setup

1. **Create Directory Structure**
   ```bash
   mkdir -p project-name/{.ai-trackdown,tasks/{epics,issues,tasks},docs,templates}
   ```

2. **Copy Template Files**
   - Copy epic-template.md to `templates/`
   - Copy issue-template.md to `templates/`
   - Copy task-template.md to `templates/`
   - Copy TASKTRACK-template.md to root as `TASKTRACK.md`

3. **Create Configuration**
   Create `.ai-trackdown/config.yaml`:
   ```yaml
   version: 1
   project:
     name: "My Project"
     id_prefix: "PROJ"
   ```

4. **Initialize llms.txt**
   Create `.ai-trackdown/llms.txt` using the basic template

---

## Task Creation Workflows

### Creating an Epic

1. **Determine Epic ID**
   - Check existing epics: `ls tasks/epics/`
   - Use next sequential number: `EPIC-001`, `EPIC-002`, etc.

2. **Create Epic File**
   ```bash
   cp templates/epic-template.md tasks/epics/EPIC-001-user-authentication.md
   ```

3. **Fill in Epic Details**
   - Update frontmatter with actual values
   - Replace placeholder text with real content
   - Set realistic target dates and token budgets
   - Define success metrics and scope

4. **Example Epic Creation**
   ```markdown
   ---
   id: EPIC-001
   type: epic
   title: User Authentication System
   status: planning
   owner: @teamlead
   created: 2025-07-07T10:00:00Z
   updated: 2025-07-07T10:00:00Z
   target_date: 2025-08-15T00:00:00Z
   labels: [authentication, security, phase-1]
   token_budget: 50000
   token_usage:
     total: 0
     remaining: 50000
   ---
   
   # User Authentication System
   
   ## Executive Summary
   Implement a comprehensive authentication system supporting email/password login, OAuth2 integration, and role-based permissions.
   
   [... continue filling template ...]
   ```

### Creating an Issue

1. **Determine Parent Epic**
   - Review existing epics in `tasks/epics/`
   - Identify which epic this issue belongs to

2. **Generate Issue ID**
   - Check existing issues: `ls tasks/issues/`
   - Use next sequential number

3. **Create Issue File**
   ```bash
   cp templates/issue-template.md tasks/issues/ISSUE-001-login-flow.md
   ```

4. **Fill in Issue Details**
   ```markdown
   ---
   id: ISSUE-001
   type: issue
   title: Implement user login flow
   status: open
   epic: EPIC-001
   assignee: @johndoe
   created: 2025-07-07T11:00:00Z
   updated: 2025-07-07T11:00:00Z
   labels: [authentication, frontend, backend]
   estimate: 5
   priority: high
   ---
   
   # Implement user login flow
   
   ## Description
   Create a secure login flow with email/password authentication...
   ```

5. **Update Parent Epic**
   - Open the parent epic file
   - Add the new issue to the "Issues Breakdown" section:
   ```markdown
   ## Issues Breakdown
   - [ ] ISSUE-001: Implement user login flow (Status: open)
   ```

### Creating a Task

1. **Identify Parent Issue**
   - Review existing issues
   - Determine which issue this task supports

2. **Create Task File**
   ```bash
   cp templates/task-template.md tasks/tasks/TASK-001-create-login-form.md
   ```

3. **Fill in Task Details**
   ```markdown
   ---
   id: TASK-001
   type: task
   title: Create login form component
   status: open
   parents: [ISSUE-001]
   assignee: @developer
   created: 2025-07-07T12:00:00Z
   updated: 2025-07-07T12:00:00Z
   labels: [frontend, react, forms]
   estimate: 2
   ---
   ```

4. **Update Parent Issue**
   - Add task to the issue's breakdown section
   ```markdown
   ## Breakdown
   - [ ] TASK-001: Create login form component
   ```

---

## Status Management Workflows

### Updating Task Status

1. **Open Task File**
   ```bash
   vi tasks/tasks/TASK-001-create-login-form.md
   ```

2. **Update Frontmatter**
   ```markdown
   ---
   status: in-progress  # was: open
   updated: 2025-07-07T14:30:00Z
   ---
   ```

3. **Add Activity Log Entry**
   ```markdown
   ## Activity Log
   - 2025-07-07T12:00:00Z: Created by @developer
   - 2025-07-07T14:30:00Z: Started development by @developer
   ```

4. **Update Progress Indicators**
   - Check off completed acceptance criteria
   - Update file modification checklist
   - Add any implementation notes

### Completing a Task

1. **Update Task Status**
   ```markdown
   ---
   status: done
   updated: 2025-07-07T16:45:00Z
   ---
   ```

2. **Complete Activity Log**
   ```markdown
   ## Activity Log
   - 2025-07-07T12:00:00Z: Created by @developer
   - 2025-07-07T14:30:00Z: Started development by @developer
   - 2025-07-07T16:45:00Z: Completed by @developer
   ```

3. **Update Parent Issue**
   - Mark task as completed in parent issue
   ```markdown
   ## Breakdown
   - [x] TASK-001: Create login form component
   ```

4. **Check Issue Completion**
   - If all tasks are done, update issue status
   - Update parent epic progress

---

## Token Tracking Workflows

### Recording Token Usage

1. **Manual Token Logging**
   When AI assists with a task, update the token usage:
   ```markdown
   ---
   token_usage:
     total: 1247
     by_agent:
       claude: 892
       gpt4: 355
   ---
   ```

2. **Add Activity Log Entry**
   ```markdown
   ## Activity Log
   - 2025-07-07T15:30:00Z: Claude analyzed requirements (tokens: 347)
   - 2025-07-07T16:00:00Z: GPT-4 generated code skeleton (tokens: 545)
   ```

3. **Update Epic Token Budget**
   ```markdown
   ---
   token_usage:
     total: 12847
     remaining: 37153
   ---
   
   ## Token Tracking
   | Date | Agent | Task | Tokens | Purpose |
   |------|-------|------|--------|---------|
   | 2025-07-07 | Claude | TASK-001 | 892 | Requirements analysis |
   ```

### Monitoring Token Budgets

1. **Weekly Token Review**
   - Review all active epics
   - Sum token usage across all issues/tasks
   - Update remaining budget calculations
   - Flag epics approaching budget limits

2. **Token Alert Process**
   ```markdown
   ## Risk Assessment
   | Risk | Impact | Probability | Mitigation |
   |------|---------|-------------|------------|
   | Token budget overrun | High | Medium | Reduce AI assistance scope |
   ```

---

## Relationship Management

### Managing Dependencies

1. **Task Dependencies**
   ```markdown
   ## Related Tasks
   - Blocks: [TASK-003, TASK-004]
   - Blocked by: [TASK-002]
   - Related: [TASK-005]
   ```

2. **Issue Dependencies**
   ```markdown
   ## Related Issues
   - Blocks: [ISSUE-003]
   - Blocked by: [ISSUE-002]
   - Depends on: [ISSUE-001]
   ```

3. **Updating Dependent Items**
   When completing a task that blocks others:
   - Find all blocked items
   - Update their "Blocked by" lists
   - Notify assignees that blockers are resolved

### Cross-Reference Maintenance

1. **Bi-directional Updates**
   When Task A blocks Task B:
   - Task A: `Blocks: [TASK-B]`
   - Task B: `Blocked by: [TASK-A]`

2. **Consistency Checks**
   - Regularly verify all references exist
   - Ensure bi-directional relationships are maintained
   - Clean up references to deleted items

---

## Search and Discovery

### Finding Tasks by Status

```bash
# Find all open issues
grep -l "status: open" tasks/issues/*.md

# Find in-progress tasks
grep -l "status: in-progress" tasks/tasks/*.md

# Find blocked items
grep -l "status: blocked" tasks/**/*.md
```

### Finding Tasks by Assignee

```bash
# Find all tasks assigned to specific person
grep -l "assignee: @johndoe" tasks/**/*.md

# Find unassigned tasks
grep -l "assignee: @username" tasks/**/*.md | grep -v "@[a-zA-Z]"
```

### Finding Tasks by Label

```bash
# Find high-priority items
grep -r "priority-high" tasks/

# Find authentication-related tasks
grep -r "authentication" tasks/

# Find frontend tasks
grep -r "frontend" tasks/
```

### Content Search

```bash
# Search for specific functionality
grep -r "OAuth" tasks/

# Find API-related tasks
grep -r "API" tasks/

# Search acceptance criteria
grep -A 10 "Acceptance Criteria" tasks/issues/*.md
```

---

## Reporting and Analytics

### Manual Status Reports

1. **Weekly Status Report Process**
   ```bash
   # Count tasks by status
   echo "Open tasks: $(grep -l 'status: open' tasks/tasks/*.md | wc -l)"
   echo "In progress: $(grep -l 'status: in-progress' tasks/tasks/*.md | wc -l)"
   echo "Done: $(grep -l 'status: done' tasks/tasks/*.md | wc -l)"
   ```

2. **Epic Progress Tracking**
   - For each epic, count completed vs total issues
   - Calculate percentage completion
   - Update epic status accordingly

3. **Token Usage Summary**
   - Extract token usage from all tasks
   - Sum by epic and by agent
   - Calculate burn rate and remaining budget

### Project Dashboard Updates

1. **Update TASKTRACK.md Weekly**
   - Refresh active epics section
   - Update team workload
   - Add recent activity
   - Update metrics and KPIs

2. **Manual Metric Collection**
   ```bash
   # Issues created this week
   find tasks/issues -name "*.md" -newermt "1 week ago" | wc -l
   
   # Issues completed this week
   grep -l "status: done" tasks/issues/*.md | xargs grep "updated:" | grep "$(date -d '1 week ago' +%Y-%m)" | wc -l
   ```

---

## Maintenance Workflows

### Daily Maintenance

1. **Status Review**
   - Check all in-progress items for updates
   - Verify blocked items for resolution
   - Update stale task statuses

2. **Activity Logging**
   - Ensure all work has activity log entries
   - Record token usage from AI interactions
   - Update time estimates if needed

### Weekly Maintenance

1. **Cross-Reference Validation**
   - Verify all task relationships are current
   - Check that parent/child relationships are accurate
   - Clean up broken references

2. **Token Budget Review**
   - Calculate weekly token usage
   - Update epic budget tracking
   - Flag potential overruns

3. **llms.txt Updates**
   - Refresh project overview
   - Update current focus items
   - Add new key directories or files

### Monthly Maintenance

1. **Archive Completed Items**
   ```bash
   # Move completed tasks to archive
   mkdir -p archive/$(date +%Y-%m)
   find tasks/tasks -name "*.md" -exec grep -l "status: done" {} \; | xargs mv archive/$(date +%Y-%m)/
   ```

2. **Epic Cleanup**
   - Archive completed epics
   - Update remaining epic timelines
   - Review and adjust token budgets

3. **Template Updates**
   - Review template effectiveness
   - Update based on team feedback
   - Add new fields as needed

---

## Integration Workflows

### Git Integration

1. **Branch Management**
   ```bash
   # Create feature branch for issue
   git checkout -b feature/ISSUE-001-login-flow
   
   # Create task branch
   git checkout -b feature/TASK-001-login-form
   ```

2. **Commit Message Format**
   ```bash
   git commit -m "feat(ISSUE-001): Add login form component
   
   - Created LoginForm React component
   - Added form validation
   - Implemented submit handler
   
   Closes TASK-001"
   ```

3. **Status Updates from Commits**
   - When committing code, update task status
   - Add activity log entries
   - Update file modification checklists

### External System Updates

1. **GitHub Issues Sync**
   - Manually create GitHub issues for external visibility
   - Copy key information from markdown files
   - Maintain reference links

2. **Jira Integration**
   - Create Jira tickets for official tracking
   - Maintain ID mapping in sync files
   - Update status in both systems

3. **Slack/Communication Updates**
   - Share daily progress in team channels
   - Post weekly summaries
   - Alert on blockers or delays

---

## Quality Assurance

### File Validation

1. **Frontmatter Validation**
   ```bash
   # Check for required fields
   for file in tasks/**/*.md; do
     if ! grep -q "id:" "$file"; then
       echo "Missing ID in $file"
     fi
   done
   ```

2. **ID Uniqueness Check**
   ```bash
   # Find duplicate IDs
   grep -h "^id:" tasks/**/*.md | sort | uniq -d
   ```

3. **Reference Validation**
   ```bash
   # Find broken references
   grep -r "TASK-" tasks/ | grep -v "id: TASK-" | cut -d: -f2 | sort -u > refs.txt
   grep "^id:" tasks/**/*.md | cut -d: -f3 | sort -u > ids.txt
   comm -23 refs.txt ids.txt  # References without matching IDs
   ```

### Content Quality

1. **Acceptance Criteria Review**
   - Ensure all issues have measurable criteria
   - Verify criteria are complete and testable
   - Check for missing edge cases

2. **Context Quality Check**
   - Verify AI context sections are current
   - Ensure technical details are accurate
   - Update outdated information

3. **Documentation Completeness**
   - Check that all files have activity logs
   - Verify token usage is recorded
   - Ensure relationships are documented

---

This manual workflow guide provides a comprehensive foundation for managing ai-trackdown projects using only text editors, basic command-line tools, and git. The processes are designed to scale from small teams to larger projects while maintaining the simplicity and transparency of the markdown-based approach.
