# AI-Trackdown Template Reference

Complete template and configuration reference for AI-Trackdown documentation framework.

## Setup and Configuration

### Template Setup
```bash
# Copy templates to your project
cp -r /path/to/ai-trackdown/templates/* .ai-trackdown/templates/

# Download templates from repository
wget https://github.com/your-org/ai-trackdown/archive/templates.zip
unzip templates.zip -d .ai-trackdown/templates/
```

### Project Configuration
```yaml
# .ai-trackdown/config.yaml
project:
  name: "My Project"
  token_budget: 50000
  default_assignee: "@team-lead"
  
templates:
  epic: ".ai-trackdown/templates/epic.md"
  issue: ".ai-trackdown/templates/issue.md"
  task: ".ai-trackdown/templates/task.md"

integrations:
  github: ".ai-trackdown/integrations/github.yaml"
  jira: ".ai-trackdown/integrations/jira.yaml"
```

## Template Usage Patterns

### Status and Information

#### Project Status Dashboard
Maintain project status in AI-TRACKDOWN.md using this structure:

```markdown
# Project Status Dashboard

## Overview
- **Project:** My AI Project
- **Phase:** Development
- **Budget Used:** 12,847 / 50,000 tokens (25.7%)

## Active Epics
- EPIC-001: User Authentication (in-progress, 70% complete)
- EPIC-002: Payment Integration (todo)

## Current Sprint
- 3 issues in progress
- 2 issues in review  
- 5 issues completed this week
```

#### Task Filtering and Queries
Use bash commands to filter and query tasks:

```bash
# List all open issues
grep -l "status: open" tasks/issues/*.md

# Find your assigned tasks
grep -l "assignee: @me" tasks/**/*.md | grep "status: in-progress"

# High priority items
grep -l "priority: high" tasks/**/*.md

# Recent items
find tasks/ -name "*.md" -newermt "2025-01-01" | head -10

# Parse YAML frontmatter for reporting
python -c "
import yaml, glob
for f in glob.glob('tasks/**/*.md', recursive=True):
    with open(f) as file:
        content = file.read()
        if content.startswith('---'):
            yaml_part = content.split('---')[1]
            data = yaml.safe_load(yaml_part)
            print(f'{data.get(\"id\")}: {data.get(\"status\")} - {data.get(\"title\")}')
"
```

### Creating Items Using Templates

#### Epic Template Usage
Create epics by copying and customizing the epic template:

```bash
# Copy epic template
cp .ai-trackdown/templates/epic.md tasks/epics/EPIC-001-user-authentication-system.md

# Edit the template
vim tasks/epics/EPIC-001-user-authentication-system.md
```

**Epic Template Structure:**
```yaml
---
id: EPIC-001
type: epic
title: User Authentication System
status: todo
owner: "@team-lead"
target_date: "2025-03-01"
token_budget: 50000
labels: [authentication, security, high-priority]
created: "2025-01-07T10:00:00Z"
updated: "2025-01-07T10:00:00Z"
---

# User Authentication System

## Description
Comprehensive authentication system supporting multiple providers.

## Success Criteria
- [ ] OAuth2 integration
- [ ] Password reset flow
- [ ] Two-factor authentication
- [ ] Security audit completed

## Token Budget Allocation
- Requirements analysis: 10,000 tokens
- Implementation: 25,000 tokens
- Testing & validation: 15,000 tokens

## AI Context
<!-- AI_CONTEXT_START -->
Authentication system for SaaS application. Supports OAuth2, password reset, and 2FA.
Key components: auth service, user management, session handling.
Security requirements: OWASP compliance, secure token storage, audit logging.
<!-- AI_CONTEXT_END -->
```

#### Issue Template Usage
Create issues by copying and customizing the issue template:

```bash
# Copy issue template  
cp .ai-trackdown/templates/issue.md tasks/issues/ISSUE-001-implement-oauth2-login.md

# Customize for specific requirements
vim tasks/issues/ISSUE-001-implement-oauth2-login.md
```

**Issue Template Structure:**
```yaml
---
id: ISSUE-001
type: issue
title: Implement OAuth2 login
status: todo
epic: EPIC-001
assignee: "@developer"
estimate: 8
priority: high
labels: [authentication, oauth, frontend]
created: "2025-01-07T10:00:00Z"
updated: "2025-01-07T10:00:00Z"
token_usage:
  total: 0
  by_agent: {}
---

# Implement OAuth2 login

## Description
Implement secure OAuth2 authentication flow supporting Google and GitHub providers.

## Acceptance Criteria
- [ ] OAuth2 provider configuration
- [ ] Authorization code flow implementation
- [ ] Token validation and refresh
- [ ] User profile integration
- [ ] Security audit completed

## Dependencies
- Requires: EPIC-001 setup
- Blocks: TASK-002, TASK-003

## AI Context
<!-- AI_CONTEXT_START -->
OAuth2 implementation for user authentication. Uses PKCE flow for security.
Files: src/auth/oauth.js, components/OAuthButton.tsx
Dependencies: passport-oauth2, jsonwebtoken
<!-- AI_CONTEXT_END -->
```

#### Task Template Usage
Create tasks by copying and customizing the task template:

```bash
# Copy task template
cp .ai-trackdown/templates/task.md tasks/tasks/TASK-001-create-login-form.md

# Add implementation details
vim tasks/tasks/TASK-001-create-login-form.md
```

**Task Template Structure:**
```yaml
---
id: TASK-001
type: task
title: Create login form component
status: todo
issue: ISSUE-001
assignee: "@frontend-dev"
estimate: 4
labels: [frontend, component]
created: "2025-01-07T10:00:00Z"
updated: "2025-01-07T10:00:00Z"
---

# Create login form component

## Description
Create reusable React login form component with OAuth2 integration.

## Implementation Details
- React functional component with hooks
- Form validation using Formik
- OAuth2 button integration
- Responsive design

## Definition of Done
- [ ] Component implemented
- [ ] Unit tests written
- [ ] Storybook documentation
- [ ] Accessibility compliance
```

### Updating Items Manually

#### File-Based Updates
Update task properties by editing the YAML frontmatter:

```bash
# Edit task file directly
vim tasks/issues/ISSUE-001-implement-oauth2-login.md

# Update specific fields in the frontmatter:
# status: todo → in-progress
# assignee: "@developer"
# priority: high
# updated: "2025-01-07T14:30:00Z"
```

**Status Update Example:**
```yaml
---
id: ISSUE-001
status: in-progress  # Changed from 'todo'
assignee: "@developer"  # Updated assignee
updated: "2025-01-07T14:30:00Z"  # Updated timestamp
---
```

**Adding Comments:**
Add a comments section in the markdown body:

```markdown
## Progress Updates
- 2025-01-07: Started OAuth2 implementation
- 2025-01-07: Ready for review - @developer
```

#### Assignment Management
```bash
# Update assignee in file
sed -i 's/assignee: ".*"/assignee: "@new-developer"/' tasks/issues/ISSUE-001-*.md

# Bulk assignment updates
for file in tasks/issues/ISSUE-*.md; do
    sed -i 's/assignee: "@old-dev"/assignee: "@new-dev"/' "$file"
done
```

#### Closing/Completing Items
```bash
# Update status to done
vim tasks/issues/ISSUE-001-implement-oauth2-login.md
# Change status: in-progress → done
# Add completion notes in Progress Updates section
```

## Token Tracking

### Manual Token Recording
Update token usage in task files:

```yaml
# In task frontmatter
token_usage:
  total: 1247
  by_agent:
    claude: 892
    gpt4: 355
  sessions:
    - date: "2025-01-07"
      agent: claude
      count: 1247
      purpose: "Requirements analysis and API design"
      cost_center: "backend-development"
```

### Token Reporting
Create reports by parsing task files:

```bash
# Extract all token usage
grep -r "token_usage:" tasks/ | head -10

# Parse token data with Python
python -c "
import yaml, glob
total_tokens = 0
by_agent = {}
for f in glob.glob('tasks/**/*.md', recursive=True):
    with open(f) as file:
        content = file.read()
        if '---' in content:
            try:
                yaml_part = content.split('---')[1]
                data = yaml.safe_load(yaml_part)
                if 'token_usage' in data:
                    usage = data['token_usage']
                    total_tokens += usage.get('total', 0)
                    for agent, count in usage.get('by_agent', {}).items():
                        by_agent[agent] = by_agent.get(agent, 0) + count
            except: pass
print(f'Total tokens: {total_tokens}')
print(f'By agent: {by_agent}')
"
```

### Budget Management
Track budgets in epic files and configuration:

```yaml
# In epic files
token_budget: 50000
token_usage:
  total: 12847
  percentage: 25.7
  alert_threshold: 0.8

# In config.yaml
budgets:
  EPIC-001: 50000
  EPIC-002: 30000
  default_epic_budget: 25000
```

## AI Context Generation

### llms.txt Maintenance
Manually maintain llms.txt files:

```bash
# Update main llms.txt
vim .ai-trackdown/llms.txt
```

**llms.txt Structure:**
```txt
# AI-Trackdown Project Index
# Generated: 2025-01-07T15:00:00Z

## Project Overview
User authentication system for SaaS application
Active epics: 1 (Authentication System)
Open issues: 3
Current focus: OAuth2 implementation

## Key Files
/tasks/epics/: Project epics and major features
/tasks/issues/: Development issues and requirements
/tasks/tasks/: Implementation tasks and subtasks
/AI-TRACKDOWN.md: Project status dashboard

## Current Priorities
1. EPIC-001: User Authentication (in-progress, 70% complete)
   - ISSUE-001: OAuth2 login (in-progress)
   - ISSUE-002: Password reset (pending)
   - ISSUE-003: Two-factor auth (pending)

## Integration Status
GitHub: Manually synced (2025-01-07)
Token Budget: 50,000 (25.7% used)
```

### Context for AI Agents
Create context by reading specific files:

```bash
# Generate context for specific work
cat tasks/epics/EPIC-001-*.md tasks/issues/ISSUE-001-*.md

# AI agents can read context directly
cat .ai-trackdown/llms.txt

# Include dependency context
grep -A 5 "Dependencies" tasks/issues/ISSUE-001-*.md
```

## Integration Configuration

### GitHub Integration Templates
```yaml
# .ai-trackdown/integrations/github.yaml
repository: myorg/myrepo
sync_enabled: false  # Manual sync only
mapping:
  issue:
    title: title
    body: |
      ${description}
      
      ## Acceptance Criteria
      ${acceptance_criteria}
    labels: labels
    assignees: 
      - transform: strip_at(assignee)
```

### Jira Integration Templates
```yaml
# .ai-trackdown/integrations/jira.yaml
server: https://company.atlassian.net
project: AUTH
sync_enabled: false  # Manual sync only
mapping:
  story_points: estimate
  epic_link: epic
  status_mapping:
    todo: "To Do"
    in-progress: "In Progress"
    in-review: "In Review"
    done: "Done"
```

### Manual Sync Workflows
```bash
# Export data for manual sync
python -c "
import yaml, glob, json
issues = []
for f in glob.glob('tasks/issues/*.md'):
    with open(f) as file:
        content = file.read()
        if '---' in content:
            yaml_part = content.split('---')[1]
            body = '---'.join(content.split('---')[2:])
            data = yaml.safe_load(yaml_part)
            data['body'] = body.strip()
            issues.append(data)
print(json.dumps(issues, indent=2))
" > export.json

# Use export.json for manual import to external systems
```

## Validation and Maintenance

### File Validation
```bash
# Validate YAML frontmatter
for file in tasks/**/*.md; do
    echo "Validating $file"
    python -c "
import yaml
with open('$file') as f:
    content = f.read()
    if content.startswith('---'):
        yaml_part = content.split('---')[1]
        try:
            yaml.safe_load(yaml_part)
            print('✓ Valid YAML')
        except Exception as e:
            print(f'✗ Invalid YAML: {e}')
    else:
        print('✗ No YAML frontmatter')
"
done
```

### Data Integrity
```bash
# Check for missing required fields
grep -L "^id:" tasks/**/*.md
grep -L "^type:" tasks/**/*.md  
grep -L "^title:" tasks/**/*.md

# Verify epic-issue relationships
python -c "
import yaml, glob
epics = set()
issues_with_epics = set()

# Collect epic IDs
for f in glob.glob('tasks/epics/*.md'):
    with open(f) as file:
        content = file.read()
        if '---' in content:
            data = yaml.safe_load(content.split('---')[1])
            epics.add(data.get('id'))

# Check issue references
for f in glob.glob('tasks/issues/*.md'):
    with open(f) as file:
        content = file.read()
        if '---' in content:
            data = yaml.safe_load(content.split('---')[1])
            epic = data.get('epic')
            if epic and epic not in epics:
                print(f'Orphaned issue {data.get(\"id\")} references missing epic {epic}')
"
```

### Import/Export

#### Export Data
```bash
# Export all tasks to JSON
python -c "
import yaml, glob, json
data = {'epics': [], 'issues': [], 'tasks': []}

for category in ['epics', 'issues', 'tasks']:
    for f in glob.glob(f'tasks/{category}/*.md'):
        with open(f) as file:
            content = file.read()
            if '---' in content:
                yaml_part = content.split('---')[1]
                body = '---'.join(content.split('---')[2:])
                item = yaml.safe_load(yaml_part)
                item['body'] = body.strip()
                data[category].append(item)

print(json.dumps(data, indent=2))
" > project-export.json
```

#### Import Data
```bash
# Import from JSON (create files from data)
python -c "
import json, yaml, os
with open('project-export.json') as f:
    data = json.load(f)

for category in ['epics', 'issues', 'tasks']:
    os.makedirs(f'tasks/{category}', exist_ok=True)
    for item in data[category]:
        body = item.pop('body', '')
        filename = f'tasks/{category}/{item[\"id\"]}-{item[\"title\"].lower().replace(\" \", \"-\")}.md'
        with open(filename, 'w') as f:
            f.write('---\n')
            f.write(yaml.dump(item))
            f.write('---\n\n')
            f.write(body)
"
```

## Configuration Reference

### Global Configuration
```yaml
# .ai-trackdown/config.yaml
project:
  name: "My AI Project"
  description: "AI-native task management"
  token_budget: 100000
  default_assignee: "@team-lead"

templates:
  epic: ".ai-trackdown/templates/epic.md"
  issue: ".ai-trackdown/templates/issue.md"  
  task: ".ai-trackdown/templates/task.md"
  security_review: ".ai-trackdown/templates/security-review.md"

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

reporting:
  token_costs:
    claude: 0.000003  # per token
    gpt4: 0.00003     # per token
    gpt3: 0.000002    # per token
```

### Environment Variables
```bash
# Optional environment configuration
export AI_TRACKDOWN_CONFIG=".ai-trackdown/config.yaml"
export GITHUB_TOKEN="your-github-token"
export JIRA_TOKEN="your-jira-token"
```

## Manual Workflow Examples

### Daily Developer Workflow
```bash
# Check your assigned work
grep -l "assignee: @me" tasks/**/*.md | grep -v "status: done"

# Start working on a task
vim tasks/issues/ISSUE-001-implement-oauth2-login.md
# Update status to 'in-progress'

# Record AI assistance manually
vim tasks/issues/ISSUE-001-implement-oauth2-login.md
# Add token usage in frontmatter

# Complete the task
git commit -m "feat(ISSUE-001): implement OAuth2 flow"
vim tasks/issues/ISSUE-001-implement-oauth2-login.md
# Update status to 'done' and add completion notes
```

### Sprint Planning
```bash
# Create new epic
cp .ai-trackdown/templates/epic.md tasks/epics/EPIC-002-payment-integration.md
vim tasks/epics/EPIC-002-payment-integration.md

# Break down into issues
cp .ai-trackdown/templates/issue.md tasks/issues/ISSUE-004-stripe-integration.md
cp .ai-trackdown/templates/issue.md tasks/issues/ISSUE-005-payment-ui.md

# Update project dashboard
vim AI-TRACKDOWN.md
```

### Weekly Reporting
```bash
# Generate token usage report
grep -r "token_usage:" tasks/ | python -c "
import sys, yaml
total = 0
for line in sys.stdin:
    # Parse token usage from grep output
    pass
print(f'Weekly token usage: {total}')
"

# Update project status
vim AI-TRACKDOWN.md

# Export for external reporting
python export_script.py > weekly-report.json
```

This template reference provides comprehensive guidance for using AI-Trackdown as a documentation framework rather than a CLI tool, focusing on manual workflows and template-based management.