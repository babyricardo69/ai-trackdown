# ai-trackdown Directory Structure Guide

This document outlines the recommended directory structure for ai-trackdown projects and explains how to organize your markdown-based task management system.

---

## Standard Project Structure

```
project-root/
├── .ai-trackdown/
│   ├── config.yaml              # Project configuration
│   ├── sync/                    # External system sync data
│   │   ├── github.json         # GitHub Issues mapping
│   │   ├── jira.json           # Jira integration data
│   │   └── linear.json         # Linear workspace data
│   └── llms.txt                 # Auto-generated AI index
├── tasks/
│   ├── epics/
│   │   ├── EPIC-001-user-authentication.md
│   │   ├── EPIC-002-payment-integration.md
│   │   └── EPIC-003-api-redesign.md
│   ├── issues/
│   │   ├── ISSUE-001-login-flow.md
│   │   ├── ISSUE-002-password-reset.md
│   │   ├── ISSUE-003-oauth-integration.md
│   │   └── ISSUE-004-payment-webhook.md
│   └── tasks/
│       ├── TASK-001-create-login-form.md
│       ├── TASK-002-implement-validation.md
│       ├── TASK-003-add-oauth-buttons.md
│       └── TASK-004-setup-webhook-endpoint.md
├── docs/
│   ├── llms-full.txt            # Complete project context for AI
│   ├── requirements.md          # Project requirements
│   ├── architecture.md          # Technical architecture
│   └── workflows.md             # Team workflows
├── templates/                   # Template files (optional)
│   ├── epic-template.md
│   ├── issue-template.md
│   └── task-template.md
└── AI-TRACKDOWN.md                 # Project overview dashboard
```

---

## Directory Purposes

### `.ai-trackdown/` - Configuration

This hidden directory contains all configuration and system files:

- **`config.yaml`**: Main project configuration
- **`sync/`**: Contains sync state for external integrations
- **`llms.txt`**: Auto-generated index file for AI discovery

### `tasks/` - Core Task Management

The main task hierarchy organized by scope:

#### `tasks/epics/`
- Contains high-level project initiatives
- Each epic represents a major feature or milestone
- Naming convention: `EPIC-XXX-descriptive-name.md`
- Typically spans multiple sprints/months

#### `tasks/issues/`
- Contains development issues and user stories
- Each issue represents a specific feature or bug fix
- Naming convention: `ISSUE-XXX-descriptive-name.md`
- Typically completed within 1-2 sprints

#### `tasks/tasks/`
- Contains granular implementation tasks
- Each task represents a specific piece of work
- Naming convention: `TASK-XXX-descriptive-name.md`
- Typically completed within hours/days

### `docs/` - Documentation

- **`llms-full.txt`**: Complete project context for AI agents
- **Additional docs**: Project-specific documentation

### `AI-TRACKDOWN.md` - Project Dashboard

Main project overview and status dashboard accessible from the root.

---

## File Naming Conventions

### ID Format
- **Epic**: `EPIC-XXX` (where XXX is zero-padded number)
- **Issue**: `ISSUE-XXX`
- **Task**: `TASK-XXX`

### File Names
- Format: `{TYPE}-{NUMBER}-{descriptive-kebab-case-name}.md`
- Examples:
  - `EPIC-001-user-authentication-system.md`
  - `ISSUE-045-implement-two-factor-auth.md`
  - `TASK-123-add-login-button-component.md`

### Descriptive Names
- Use kebab-case (lowercase with hyphens)
- Be descriptive but concise
- Avoid abbreviations unless universally understood
- Include key domain concepts

---

## Organization Best Practices

### 1. Hierarchical Relationships

```
EPIC-001 (User Authentication)
├── ISSUE-001 (Login Flow)
│   ├── TASK-001 (Create Login Form)
│   ├── TASK-002 (Add Validation)
│   └── TASK-003 (Implement Submit)
├── ISSUE-002 (Password Reset)
│   ├── TASK-004 (Reset Form)
│   └── TASK-005 (Email Integration)
└── ISSUE-003 (OAuth Integration)
    ├── TASK-006 (Google OAuth)
    └── TASK-007 (GitHub OAuth)
```

### 2. Status-Based Organization

While files remain in their type-based directories, use status filtering:

- **Active Work**: Issues/tasks with status `in-progress`
- **Backlog**: Items with status `open` or `todo`
- **Completed**: Items with status `done` or `closed`
- **Blocked**: Items with status `blocked`

### 3. Sprint/Milestone Organization

Use labels and metadata rather than directory structure:

```yaml
labels: [sprint-12, milestone-v2.0, priority-high]
```

### 4. Team Assignment

Assign ownership through metadata:

```yaml
assignee: @johndoe
team: frontend
reviewer: @janedoe
```

---

## Workflow Integration

### Git Integration

#### Branch Naming
Align branch names with task IDs:
```
feature/ISSUE-001-login-flow
bugfix/TASK-042-fix-validation
epic/EPIC-003-api-redesign
```

#### Commit Messages
Reference task IDs in commits:
```
feat(ISSUE-001): Add login form component
fix(TASK-042): Resolve validation edge case
docs(EPIC-003): Update API documentation
```

### Status Updates

Update task status through file modifications:
1. Edit the frontmatter status field
2. Add activity log entries
3. Update progress indicators
4. Commit changes to git

---

## Scaling Considerations

### Large Projects (1000+ tasks)

#### Monthly Archiving
```
tasks/
├── active/           # Current active tasks
├── 2025-01/         # January 2025 archive
├── 2025-02/         # February 2025 archive
└── archive/         # Older completed tasks
```

#### Component-Based Organization
```
tasks/
├── epics/
├── frontend/
│   ├── issues/
│   └── tasks/
├── backend/
│   ├── issues/
│   └── tasks/
└── infrastructure/
    ├── issues/
    └── tasks/
```

### Multi-Project Workspaces

```
workspace/
├── project-a/
│   └── [full ai-trackdown structure]
├── project-b/
│   └── [full ai-trackdown structure]
└── shared/
    ├── templates/
    └── common-docs/
```

---

## Search and Discovery

### File-Based Search

#### Find by Status
```bash
# Find all open issues
grep -l "status: open" tasks/issues/*.md

# Find tasks assigned to specific user
grep -l "assignee: @johndoe" tasks/**/*.md
```

#### Find by Content
```bash
# Search for authentication-related tasks
grep -r "authentication" tasks/

# Find all blocked items
grep -l "status: blocked" tasks/**/*.md
```

### Metadata Queries

#### Find by Labels
```bash
# Find high-priority items
grep -l "priority-high" tasks/**/*.md

# Find sprint-specific tasks
grep -l "sprint-12" tasks/**/*.md
```

---

## Maintenance Guidelines

### Regular Cleanup

1. **Monthly Reviews**
   - Archive completed tasks older than 3 months
   - Update stale task statuses
   - Verify epic progress tracking

2. **Weekly Updates**
   - Update AI-TRACKDOWN.md dashboard
   - Regenerate llms.txt index
   - Sync with external systems

3. **Daily Maintenance**
   - Update active task statuses
   - Log time and token usage
   - Update activity logs

### File Integrity

1. **Validate Frontmatter**
   - Ensure all required fields present
   - Verify date formats
   - Check ID uniqueness

2. **Link Validation**
   - Verify task relationships
   - Check epic/issue associations
   - Validate external references

3. **Content Quality**
   - Ensure acceptance criteria completeness
   - Verify token context accuracy
   - Update outdated technical information

---

## Integration with External Tools

### IDE Integration

Most modern IDEs support markdown and can be enhanced:

1. **VS Code Extensions**
   - Markdown preview
   - YAML frontmatter support
   - File explorer organization

2. **Search Integration**
   - Full-text search across all files
   - Metadata-based filtering
   - Quick file navigation

### External System Sync

When syncing with external systems:

1. **Preserve Local Structure**
   - Keep the directory organization intact
   - Maintain local task relationships
   - Store sync metadata separately

2. **Conflict Resolution**
   - Local files are source of truth
   - External changes trigger manual review
   - Maintain audit trail of all changes

---

This directory structure provides a solid foundation for managing tasks in a markdown-based system while maintaining flexibility for project-specific adaptations.
