# AI Track Down Framework Specification

**Version:** 1.0  
**Status:** Framework Specification  
**Last Updated:** January 2025

## Table of Contents

1. [Overview](#overview)
2. [Setup & Configuration](#setup--configuration)
3. [Template Reference](#template-reference)
4. [File Structure Standards](#file-structure-standards)
5. [YAML Schema Reference](#yaml-schema-reference)
6. [Integration Patterns](#integration-patterns)
7. [Validation Rules](#validation-rules)
8. [Manual Workflow Specifications](#manual-workflow-specifications)

---

## Overview

AI Track Down is a documentation framework for managing AI-native task tracking through markdown files and YAML frontmatter. It provides standardized templates, configurations, and manual workflows for team collaboration.

### Design Principles

- **Template-Driven**: All task items use standardized markdown templates
- **Git-Native**: Files live alongside code in version control
- **AI-Friendly**: Structured format optimized for AI agent consumption
- **Human-Readable**: Pure markdown readable in any editor
- **Framework-Agnostic**: No runtime dependencies or special tooling

### Framework Structure

```
project/
├── .ai-trackdown/
│   ├── config.yaml           # Project configuration
│   ├── llms.txt             # AI context index
│   ├── templates/           # Task templates
│   └── integrations/        # External system configs
├── tasks/
│   ├── epics/               # High-level project goals
│   ├── issues/              # Development issues/features
│   └── tasks/               # Granular implementation tasks
├── docs/
│   └── llms-full.txt        # Complete project context
└── AI-TRACKDOWN.md          # Project dashboard
```

---

## Setup & Configuration

### Initial Setup

#### Template Installation
```bash
# Copy framework templates to your project
cp -r /path/to/AI-Track-Down/templates/* .ai-trackdown/templates/

# Download from repository
wget https://github.com/your-org/AI-Track-Down/archive/templates.zip
unzip templates.zip -d .ai-trackdown/templates/

# Create directory structure
mkdir -p .ai-trackdown/templates .ai-trackdown/integrations
mkdir -p tasks/epics tasks/issues tasks/tasks docs
```

#### Project Configuration

Create `.ai-trackdown/config.yaml`:

```yaml
# .ai-trackdown/config.yaml
project:
  name: "My AI Project"
  description: "AI-native task management"
  id_prefix: "PROJ"
  token_budget: 100000
  default_assignee: "@team-lead"

templates:
  epic: ".ai-trackdown/templates/epic.md"
  issue: ".ai-trackdown/templates/issue.md"
  task: ".ai-trackdown/templates/task.md"
  
workflows:
  statuses: [todo, in-progress, in-review, done]
  labels: [feature, bug, enhancement, security, documentation]
  priorities: [low, medium, high, critical]

integrations:
  github:
    enabled: false
    config: ".ai-trackdown/integrations/github.yaml"
  jira:
    enabled: false
    config: ".ai-trackdown/integrations/jira.yaml"
```

#### Verification

```bash
# Validate setup
ls .ai-trackdown/templates/
cat .ai-trackdown/config.yaml | head -10

# Create test epic
cp .ai-trackdown/templates/epic.md tasks/epics/EPIC-001-test.md
vim tasks/epics/EPIC-001-test.md
```

---

## Template Reference

### Epic Template Specification

**File:** `.ai-trackdown/templates/epic.md`

```yaml
---
id: EPIC-XXX
type: epic
title: "[Epic Title]"
status: todo
owner: "@owner"
target_date: "YYYY-MM-DD"
token_budget: 50000
token_usage:
  total: 0
  percentage: 0.0
  alert_threshold: 0.8
labels: []
created: "YYYY-MM-DDTHH:MM:SSZ"
updated: "YYYY-MM-DDTHH:MM:SSZ"
---

# [Epic Title]

## Description
[Comprehensive description of the epic scope and objectives]

## Success Criteria
- [ ] [Measurable success criterion 1]
- [ ] [Measurable success criterion 2]
- [ ] [Measurable success criterion 3]

## Token Budget Allocation
- Requirements analysis: X,XXX tokens
- Implementation: XX,XXX tokens  
- Testing & validation: X,XXX tokens
- Documentation: X,XXX tokens

## Dependencies
- **Requires:** [Prerequisites or dependencies]
- **Blocks:** [What this epic blocks]
- **Related:** [Related epics or projects]

## AI Context
<!-- AI_CONTEXT_START -->
[Context for AI agents: purpose, key components, technical requirements]
<!-- AI_CONTEXT_END -->

## Progress Tracking
- [ ] Epic planning complete
- [ ] Issues created and estimated
- [ ] Implementation started
- [ ] Testing phase
- [ ] Documentation complete
- [ ] Epic review and closure
```

### Issue Template Specification

**File:** `.ai-trackdown/templates/issue.md`

```yaml
---
id: ISSUE-XXX
type: issue
title: "[Issue Title]"
status: todo
epic: EPIC-XXX
assignee: "@assignee"
estimate: 0
priority: medium
labels: []
created: "YYYY-MM-DDTHH:MM:SSZ"
updated: "YYYY-MM-DDTHH:MM:SSZ"
token_usage:
  total: 0
  by_agent: {}
  sessions: []
sync:
  github: null
  jira: null
---

# [Issue Title]

## Description
[Detailed description of the issue, feature, or requirement]

## Acceptance Criteria
- [ ] [Specific, testable acceptance criterion 1]
- [ ] [Specific, testable acceptance criterion 2]
- [ ] [Specific, testable acceptance criterion 3]

## Technical Requirements
[Technical specifications, constraints, or requirements]

## Dependencies
- **Requires:** [Prerequisites]
- **Blocks:** [What this blocks]
- **Related:** [Related issues or tasks]

## AI Context
<!-- AI_CONTEXT_START -->
[Context for AI agents: relevant files, dependencies, technical considerations]
<!-- AI_CONTEXT_END -->

## Definition of Done
- [ ] Implementation complete
- [ ] Unit tests written and passing
- [ ] Integration tests passing
- [ ] Code review completed
- [ ] Documentation updated
- [ ] Acceptance criteria verified

## Progress Updates
[Add timestamped progress updates here]
```

### Task Template Specification

**File:** `.ai-trackdown/templates/task.md`

```yaml
---
id: TASK-XXX
type: task
title: "[Task Title]"
status: todo
issue: ISSUE-XXX
assignee: "@assignee"
estimate: 0
labels: []
created: "YYYY-MM-DDTHH:MM:SSZ"
updated: "YYYY-MM-DDTHH:MM:SSZ"
token_usage:
  total: 0
  by_agent: {}
---

# [Task Title]

## Description
[Specific, actionable description of the task]

## Implementation Details
[Technical implementation specifics, approach, or methodology]

## Acceptance Criteria
- [ ] [Specific criterion 1]
- [ ] [Specific criterion 2]
- [ ] [Specific criterion 3]

## Dependencies
- **Requires:** [Prerequisites]
- **Files:** [Relevant files or components]

## Definition of Done
- [ ] Task implementation complete
- [ ] Testing completed
- [ ] Code review passed
- [ ] Documentation updated (if needed)

## Progress Notes
[Add implementation notes and progress updates]
```

---

## File Structure Standards

### Naming Conventions

#### File Names
```
tasks/epics/EPIC-{number}-{slug}.md
tasks/issues/ISSUE-{number}-{slug}.md
tasks/tasks/TASK-{number}-{slug}.md

Examples:
- EPIC-001-user-authentication-system.md
- ISSUE-001-implement-oauth2-login.md
- TASK-001-create-login-form-component.md
```

#### ID Patterns
```
EPIC-{sequential_number}
ISSUE-{sequential_number}
TASK-{sequential_number}

Or with custom prefix:
{PREFIX}-EPIC-{number}
{PREFIX}-ISSUE-{number}
{PREFIX}-TASK-{number}
```

### Directory Structure Requirements

```
.ai-trackdown/
├── config.yaml              # Required: Project configuration
├── llms.txt                  # Required: AI context index
├── templates/                # Required: Template directory
│   ├── epic.md              # Required: Epic template
│   ├── issue.md             # Required: Issue template
│   ├── task.md              # Required: Task template
│   └── custom-*.md          # Optional: Custom templates
└── integrations/            # Optional: External integrations
    ├── github.yaml          # Optional: GitHub configuration
    ├── jira.yaml            # Optional: Jira configuration
    └── linear.yaml          # Optional: Linear configuration

tasks/
├── epics/                   # Required: Epic files
├── issues/                  # Required: Issue files
└── tasks/                   # Required: Task files

docs/                        # Optional: Documentation
└── llms-full.txt           # Optional: Full context file

AI-TRACKDOWN.md              # Required: Project dashboard
```

---

## YAML Schema Reference

### Common Fields

#### Required Fields (All Types)
```yaml
id: string                   # Unique identifier
type: enum[epic, issue, task] # Item type
title: string                # Human-readable title
status: enum[todo, in-progress, in-review, done] # Current status
created: datetime            # ISO 8601 creation timestamp
updated: datetime            # ISO 8601 last update timestamp
```

#### Optional Common Fields
```yaml
assignee: string             # @username or email
labels: array[string]        # Tag labels
description: string          # Brief description
```

### Epic-Specific Fields
```yaml
owner: string                # Epic owner (@username)
target_date: date           # Target completion (YYYY-MM-DD)
token_budget: integer       # Token allocation
token_usage:
  total: integer            # Total tokens used
  percentage: float         # Usage percentage (0.0-1.0)
  alert_threshold: float    # Alert threshold (0.0-1.0)
```

### Issue-Specific Fields
```yaml
epic: string                # Parent epic ID
estimate: integer           # Story point estimate
priority: enum[low, medium, high, critical] # Priority level
token_usage:
  total: integer            # Total tokens used
  by_agent: object          # Token usage by AI agent
  sessions: array           # Individual sessions
sync:
  github: integer/null      # GitHub issue number
  jira: string/null         # Jira ticket key
```

### Task-Specific Fields
```yaml
issue: string               # Parent issue ID
estimate: integer           # Hour estimate
token_usage:
  total: integer            # Total tokens used
  by_agent: object          # Token usage by AI agent
```

### Token Usage Schema
```yaml
token_usage:
  total: integer            # Total token count
  by_agent:
    claude: integer         # Tokens used by Claude
    gpt4: integer          # Tokens used by GPT-4
    # ... other agents
  sessions:                # Individual session records
    - date: datetime       # Session date
      agent: string        # AI agent name
      count: integer       # Token count
      purpose: string      # Session purpose
      cost_center: string  # Optional cost center
```

---

## Integration Patterns

### GitHub Integration Configuration

**File:** `.ai-trackdown/integrations/github.yaml`

```yaml
# GitHub Integration Configuration
repository: "org/repo-name"
sync_enabled: false          # Manual sync only

# Field mapping for GitHub Issues
mapping:
  issue:
    title: title
    body: |
      ${description}
      
      ## Acceptance Criteria
      ${acceptance_criteria}
      
      ## AI Context
      ${ai_context}
    labels: labels
    assignees: 
      - field: assignee
        transform: strip_at    # Remove @ prefix
    milestone:
      field: epic
      transform: epic_to_milestone

# Status mapping
status_mapping:
  todo: "open"
  in-progress: "open"  
  in-review: "open"
  done: "closed"

# Label mapping
label_mapping:
  priority-high: "priority: high"
  priority-medium: "priority: medium"
  priority-low: "priority: low"
  feature: "type: feature"
  bug: "type: bug"
```

### Jira Integration Configuration

**File:** `.ai-trackdown/integrations/jira.yaml`

```yaml
# Jira Integration Configuration
server: "https://company.atlassian.net"
project: "PROJ"
sync_enabled: false          # Manual sync only

# Field mapping for Jira Issues
mapping:
  issue:
    summary: title
    description: |
      ${description}
      
      h3. Acceptance Criteria
      ${acceptance_criteria}
    issuetype: 
      field: type
      mapping:
        issue: "Story"
        task: "Task" 
        epic: "Epic"
    assignee:
      field: assignee
      transform: strip_at
    priority:
      field: priority
      mapping:
        low: "Low"
        medium: "Medium"
        high: "High"
        critical: "Highest"

# Custom field mapping
custom_fields:
  story_points: "estimate"
  epic_link: "epic"
  
# Status mapping
status_mapping:
  todo: "To Do"
  in-progress: "In Progress"
  in-review: "In Review"
  done: "Done"
```

---

## Validation Rules

### File Validation Requirements

#### YAML Frontmatter Validation
```bash
# Required fields check
required_fields=(id type title status created updated)

# Field format validation
id_pattern="^(EPIC|ISSUE|TASK)-[0-9]+$"
datetime_pattern="^[0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{2}:[0-9]{2}:[0-9]{2}Z$"
status_values=("todo" "in-progress" "in-review" "done")
```

#### Relationship Validation
```python
# Epic-Issue relationships
def validate_epic_references():
    epics = get_epic_ids()
    for issue in get_issues():
        if issue.epic and issue.epic not in epics:
            raise ValidationError(f"Issue {issue.id} references missing epic {issue.epic}")

# Issue-Task relationships  
def validate_task_references():
    issues = get_issue_ids()
    for task in get_tasks():
        if task.issue and task.issue not in issues:
            raise ValidationError(f"Task {task.id} references missing issue {task.issue}")
```

#### Token Budget Validation
```python
def validate_token_budgets():
    for epic in get_epics():
        if epic.token_usage.total > epic.token_budget:
            raise ValidationError(f"Epic {epic.id} exceeds token budget")
        
        if epic.token_usage.percentage > epic.token_usage.alert_threshold:
            warn(f"Epic {epic.id} approaching token budget limit")
```

### Data Integrity Checks

#### File Naming Validation
```bash
# Validate file naming convention
for file in tasks/**/*.md; do
    basename=$(basename "$file" .md)
    if [[ ! $basename =~ ^(EPIC|ISSUE|TASK)-[0-9]+-[a-z0-9-]+$ ]]; then
        echo "Invalid filename: $file"
    fi
done
```

#### Duplicate ID Detection
```python
def check_duplicate_ids():
    ids = []
    for file in glob.glob('tasks/**/*.md', recursive=True):
        data = parse_yaml_frontmatter(file)
        if data['id'] in ids:
            raise ValidationError(f"Duplicate ID: {data['id']}")
        ids.append(data['id'])
```

---

## Manual Workflow Specifications

### Item Creation Workflow

#### Epic Creation Process
```bash
# 1. Copy template
cp .ai-trackdown/templates/epic.md tasks/epics/EPIC-XXX-title.md

# 2. Generate ID
next_id=$(ls tasks/epics/ | grep -E 'EPIC-[0-9]+' | \
          sed 's/.*EPIC-\([0-9]\+\).*/\1/' | sort -n | tail -1)
epic_id="EPIC-$(printf '%03d' $((next_id + 1)))"

# 3. Update template
sed -i "s/EPIC-XXX/$epic_id/g" tasks/epics/EPIC-XXX-title.md
sed -i "s/YYYY-MM-DDTHH:MM:SSZ/$(date -u +%Y-%m-%dT%H:%M:%SZ)/g" tasks/epics/EPIC-XXX-title.md

# 4. Rename file  
mv tasks/epics/EPIC-XXX-title.md tasks/epics/$epic_id-title.md

# 5. Edit content
vim tasks/epics/$epic_id-title.md
```

#### Issue Creation Process
```bash
# 1. Copy template and generate ID
cp .ai-trackdown/templates/issue.md tasks/issues/ISSUE-XXX-title.md
issue_id="ISSUE-$(printf '%03d' $(($(ls tasks/issues/ | wc -l) + 1)))"

# 2. Update template fields
sed -i "s/ISSUE-XXX/$issue_id/g" tasks/issues/ISSUE-XXX-title.md
sed -i "s/EPIC-XXX/$parent_epic/g" tasks/issues/ISSUE-XXX-title.md

# 3. Set timestamps
created_date=$(date -u +%Y-%m-%dT%H:%M:%SZ)
sed -i "s/YYYY-MM-DDTHH:MM:SSZ/$created_date/g" tasks/issues/ISSUE-XXX-title.md

# 4. Rename and edit
mv tasks/issues/ISSUE-XXX-title.md tasks/issues/$issue_id-title.md
vim tasks/issues/$issue_id-title.md
```

### Status Update Workflow

#### Manual Status Updates
```bash
# Update issue status
vim tasks/issues/ISSUE-001-implement-login.md
# Change: status: todo → status: in-progress
# Update: updated: "$(date -u +%Y-%m-%dT%H:%M:%SZ)"

# Bulk status updates
find tasks/issues/ -name "*.md" -exec grep -l "assignee: @johndoe" {} \; | \
while read file; do
    sed -i 's/status: todo/status: in-progress/' "$file"
    sed -i "s/updated: .*/updated: \"$(date -u +%Y-%m-%dT%H:%M:%SZ)\"/" "$file"
done
```

#### Git Integration Pattern
```bash
# Conventional commit with task reference
git commit -m "feat(ISSUE-001): implement OAuth2 login flow

- Add OAuth2 provider configuration  
- Implement PKCE flow for security
- Add token refresh mechanism

Token-Usage: claude=1247,gpt4=892"

# Update task status after commit
vim tasks/issues/ISSUE-001-implement-oauth2-login.md
# Add commit reference and update status
```

### Token Tracking Workflow

#### Manual Token Recording
```yaml
# Update token usage in task frontmatter
token_usage:
  total: 1247
  by_agent:
    claude: 892
    gpt4: 355
  sessions:
    - date: "2025-01-07T14:30:00Z"
      agent: claude
      count: 892
      purpose: "Implementation assistance"
      cost_center: "backend-development"
    - date: "2025-01-07T15:15:00Z"  
      agent: gpt4
      count: 355
      purpose: "Code review"
      cost_center: "quality-assurance"
```

#### Token Reporting Process
```bash
# Generate token usage report
python -c "
import yaml, glob, json
from datetime import datetime, timedelta

# Collect token usage from all tasks
total_usage = {}
weekly_usage = {}
week_ago = datetime.now() - timedelta(days=7)

for file in glob.glob('tasks/**/*.md', recursive=True):
    with open(file) as f:
        content = f.read()
        if '---' in content:
            frontmatter = content.split('---')[1]
            data = yaml.safe_load(frontmatter)
            
            if 'token_usage' in data:
                usage = data['token_usage']
                task_id = data['id']
                total_usage[task_id] = usage.get('total', 0)
                
                # Weekly tracking
                for session in usage.get('sessions', []):
                    session_date = datetime.fromisoformat(session['date'].replace('Z', '+00:00'))
                    if session_date >= week_ago:
                        agent = session['agent']
                        weekly_usage[agent] = weekly_usage.get(agent, 0) + session['count']

print(f'Total tokens: {sum(total_usage.values())}')
print(f'Weekly by agent: {weekly_usage}')
"
```

This framework specification provides comprehensive guidance for implementing AI Track Down as a documentation framework with manual workflows, templates, and configuration-driven approaches rather than CLI automation.