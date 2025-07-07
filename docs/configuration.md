# Configuration Guide

AI-Trackdown uses a YAML-based configuration system for managing project settings, templates, and integration configurations.

## Configuration File Location

The main configuration file is located at:
```
.ai-trackdown/config.yaml
```

## Basic Configuration

### Minimal Setup

```yaml
version: 1
project:
  name: "My Project"
  id_prefix: "PROJ"
  token_budget: 100000
```

This minimal configuration sets up AI-Trackdown with:
- Project name for identification
- ID prefix for generated task IDs (PROJ-EPIC-001, PROJ-ISSUE-001, etc.)
- Default token budget for epics

### Standard Configuration

```yaml
version: 1
project:
  name: "My Web Application"
  description: "AI-native task management for web development"
  id_prefix: "WEB"
  token_budget: 100000
  default_assignee: "@team-lead"
  
templates:
  epic: ".ai-trackdown/templates/epic.md"
  issue: ".ai-trackdown/templates/issue.md"
  task: ".ai-trackdown/templates/task.md"
  
workflows:
  statuses: [todo, in-progress, in-review, done, blocked]
  labels: [feature, bug, enhancement, security, documentation, performance]
  priorities: [low, medium, high, critical]
  
ai:
  llms_txt:
    auto_update: false  # Manual maintenance only
    include_closed: false
    context_depth: 3
  
integrations:
  github:
    enabled: false
    config: ".ai-trackdown/integrations/github.yaml"
  jira:
    enabled: false  
    config: ".ai-trackdown/integrations/jira.yaml"
  linear:
    enabled: false
    config: ".ai-trackdown/integrations/linear.yaml"
```

## Project Configuration

### Project Settings

```yaml
project:
  name: "Project Name"                    # Display name
  description: "Project description"      # Brief description
  id_prefix: "PROJ"                      # Task ID prefix
  token_budget: 100000                   # Default epic token budget
  default_assignee: "@team-lead"         # Default assignee for new tasks
  start_date: "2025-01-01"              # Project start date
  target_date: "2025-06-30"             # Project target completion
```

### Directory Structure

```yaml
directories:
  tasks: "tasks"                         # Task files directory
  epics: "tasks/epics"                   # Epic files subdirectory
  issues: "tasks/issues"                 # Issue files subdirectory
  tasks: "tasks/tasks"                   # Task files subdirectory
  docs: "docs"                          # Documentation directory
  templates: ".ai-trackdown/templates"   # Template directory
  integrations: ".ai-trackdown/integrations" # Integration configs
```

## Template Configuration

### Template Mapping

```yaml
templates:
  epic: ".ai-trackdown/templates/epic.md"
  issue: ".ai-trackdown/templates/issue.md"
  task: ".ai-trackdown/templates/task.md"
  
  # Custom templates
  security_review: ".ai-trackdown/templates/security-review.md"
  bug_report: ".ai-trackdown/templates/bug-report.md"
  feature_request: ".ai-trackdown/templates/feature-request.md"
```

### Template Variables

Templates support variable substitution:

```yaml
template_variables:
  default_assignee: "@team-lead"
  default_priority: "medium"
  default_labels: ["feature"]
  token_costs:
    claude: 0.000003    # Cost per token
    gpt4: 0.00003
    gpt3: 0.000002
```

## Workflow Configuration

### Status Workflow

```yaml
workflows:
  statuses: [todo, in-progress, in-review, done, blocked, cancelled]
  
  # Status transitions (for validation)
  transitions:
    todo: [in-progress, blocked, cancelled]
    in-progress: [in-review, blocked, cancelled]
    in-review: [done, in-progress, blocked]
    blocked: [todo, in-progress, cancelled]
    done: []  # Terminal state
    cancelled: []  # Terminal state
    
  # Required fields by status
  required_fields:
    in-review: [assignee, estimate]
    done: [assignee]
```

### Labels and Categories

```yaml
workflows:
  labels:
    types: [feature, bug, enhancement, chore, security, documentation]
    priorities: [priority-low, priority-medium, priority-high, priority-critical]
    components: [frontend, backend, api, database, infrastructure]
    teams: [team-core, team-platform, team-ui]
    
  priorities: [low, medium, high, critical]
  
  # Label validation rules
  label_rules:
    required_type: true          # Must have at least one type label
    max_priority: 1             # Maximum one priority label
    valid_combinations:         # Valid label combinations
      - [security, backend]
      - [performance, frontend]
```

### Estimation System

```yaml
workflows:
  estimation:
    epic_scale: "story_points"   # story_points, t_shirt, fibonacci
    issue_scale: "story_points"
    task_scale: "hours"
    
    story_points: [1, 2, 3, 5, 8, 13, 21]
    t_shirt: [XS, S, M, L, XL, XXL]
    fibonacci: [1, 1, 2, 3, 5, 8, 13, 21, 34]
```

## AI Configuration

### llms.txt Generation

```yaml
ai:
  llms_txt:
    auto_update: false           # Manual maintenance only
    include_closed: false        # Include completed tasks
    context_depth: 3            # Relationship depth
    max_file_size: "50KB"       # Maximum file size
    
    # Content sections to include
    sections:
      - project_overview
      - current_priorities  
      - key_files
      - integration_status
      - token_usage_summary
    
    # File patterns to include in context
    include_patterns:
      - "tasks/**/*.md"
      - "docs/*.md"
      - "README.md"
      - "CHANGELOG.md"
```

### Token Tracking

```yaml
ai:
  token_tracking:
    enabled: true
    budget_alerts: true
    alert_threshold: 0.8         # Alert at 80% of budget
    
    # Cost calculation
    costs:
      claude: 0.000003           # Cost per token
      gpt4: 0.00003
      gpt3: 0.000002
      codex: 0.000002
      
    # Reporting
    reports:
      daily: true
      weekly: true
      monthly: true
      format: "markdown"         # markdown, json, csv
```

### AI Context Management

```yaml
ai:
  context:
    max_context_tokens: 8000     # Maximum context size
    include_dependencies: true    # Include related tasks
    include_git_info: true       # Include git commit info
    
    # Context priorities
    priority_order:
      - current_task
      - blocking_tasks
      - related_tasks
      - epic_context
      - project_overview
```

## Integration Configuration

### GitHub Integration

```yaml
integrations:
  github:
    enabled: false
    config: ".ai-trackdown/integrations/github.yaml"
    sync_mode: "manual"          # manual, webhook, scheduled
    
    # Sync settings
    sync:
      issues: true
      pull_requests: false
      milestones: true
      labels: true
      
    # Rate limiting
    rate_limit:
      requests_per_hour: 5000
      burst_limit: 100
```

**GitHub Integration File (`.ai-trackdown/integrations/github.yaml`):**

```yaml
# GitHub Integration Configuration
repository: "org/repo-name"
authentication:
  method: "token"               # token, app
  token_env: "GITHUB_TOKEN"     # Environment variable
  
mapping:
  issue:
    title: "title"
    body: |
      ${description}
      
      ## Acceptance Criteria
      ${acceptance_criteria}
    labels: "labels"
    assignees: 
      field: "assignee"
      transform: "strip_at"
    milestone:
      field: "epic"
      transform: "epic_to_milestone"
      
  epic:
    title: "title"
    description: "description"
    
status_mapping:
  todo: "open"
  in-progress: "open"
  in-review: "open"
  done: "closed"
  
label_mapping:
  priority-high: "priority: high"
  priority-medium: "priority: medium"
  priority-low: "priority: low"
  feature: "type: feature"
  bug: "type: bug"
```

### Jira Integration

```yaml
integrations:
  jira:
    enabled: false
    config: ".ai-trackdown/integrations/jira.yaml"
    sync_mode: "manual"
```

**Jira Integration File (`.ai-trackdown/integrations/jira.yaml`):**

```yaml
# Jira Integration Configuration
server: "https://company.atlassian.net"
project: "PROJ"
authentication:
  method: "basic"               # basic, oauth, token
  username_env: "JIRA_USERNAME"
  token_env: "JIRA_TOKEN"

mapping:
  issue:
    summary: "title"
    description: |
      ${description}
      
      h3. Acceptance Criteria
      ${acceptance_criteria}
    issuetype:
      field: "type"
      mapping:
        epic: "Epic"
        issue: "Story" 
        task: "Task"
    priority:
      field: "priority"
      mapping:
        low: "Low"
        medium: "Medium"
        high: "High"
        critical: "Highest"
        
custom_fields:
  story_points: "estimate"
  epic_link: "epic"
  ai_trackdown_id: "customfield_10001"
  
status_mapping:
  todo: "To Do"
  in-progress: "In Progress"
  in-review: "In Review"
  done: "Done"
```

## Validation Configuration

### Data Validation Rules

```yaml
validation:
  strict_mode: false            # Strict validation mode
  
  required_fields:
    epic: [id, type, title, status, owner, token_budget]
    issue: [id, type, title, status, epic, assignee, estimate]
    task: [id, type, title, status, issue, assignee]
    
  field_validation:
    id_pattern: "^(EPIC|ISSUE|TASK)-[0-9]+$"
    status_values: [todo, in-progress, in-review, done, blocked, cancelled]
    priority_values: [low, medium, high, critical]
    
  relationship_validation:
    enforce_epic_issue_links: true
    enforce_issue_task_links: true
    prevent_circular_dependencies: true
    
  file_validation:
    yaml_syntax: true
    filename_pattern: true
    required_sections: true
```

### Budget Validation

```yaml
validation:
  budget:
    enforce_epic_budgets: true
    allow_budget_overrides: false
    alert_thresholds: [0.5, 0.8, 0.9]  # 50%, 80%, 90%
    
    # Budget inheritance
    issue_budget_from_epic: true
    task_budget_from_issue: false
```

## Reporting Configuration

### Report Generation

```yaml
reporting:
  formats: [markdown, json, csv, html]
  default_format: "markdown"
  
  templates:
    daily: ".ai-trackdown/reports/daily.md"
    weekly: ".ai-trackdown/reports/weekly.md"
    sprint: ".ai-trackdown/reports/sprint.md"
    
  sections:
    - project_overview
    - epic_progress
    - team_velocity
    - token_usage
    - integration_status
    - upcoming_deadlines
```

### Automated Reports

```yaml
reporting:
  automation:
    enabled: false              # Manual reporting only
    
  export:
    include_archive: false
    include_metadata: true
    compress_output: false
```

## Security Configuration

### Access Control

```yaml
security:
  file_permissions: "644"       # Default file permissions
  directory_permissions: "755"  # Default directory permissions
  
  # Sensitive data handling
  mask_tokens: true            # Mask API tokens in logs
  encrypt_credentials: false   # Encrypt stored credentials
  
  # Validation
  sanitize_inputs: true        # Sanitize YAML inputs
  validate_templates: true     # Validate template security
```

### Integration Security

```yaml
security:
  integrations:
    require_https: true         # Require HTTPS for integrations
    validate_certificates: true # Validate SSL certificates
    timeout: 30                # Request timeout in seconds
    
    # Rate limiting
    max_requests_per_minute: 60
    retry_attempts: 3
    retry_delay: 5             # Seconds between retries
```

## Environment Variables

AI-Trackdown supports environment variable override:

```bash
# Configuration
export AI_TRACKDOWN_CONFIG="/path/to/config.yaml"
export AI_TRACKDOWN_DEBUG="true"

# GitHub Integration
export GITHUB_TOKEN="your-github-token"

# Jira Integration  
export JIRA_USERNAME="your-username"
export JIRA_TOKEN="your-jira-token"

# Linear Integration
export LINEAR_TOKEN="your-linear-token"

# AI Services
export OPENAI_API_KEY="your-openai-key"
export ANTHROPIC_API_KEY="your-anthropic-key"
```

## Configuration Validation

### Manual Validation

```bash
# Validate configuration syntax
python -c "
import yaml
with open('.ai-trackdown/config.yaml') as f:
    config = yaml.safe_load(f)
    print('✓ Configuration is valid YAML')
    print(f'Project: {config.get(\"project\", {}).get(\"name\", \"Unknown\")}')
"

# Check required directories
for dir in .ai-trackdown tasks/epics tasks/issues tasks/tasks; do
    if [ -d "$dir" ]; then
        echo "✓ Directory exists: $dir"
    else
        echo "✗ Missing directory: $dir"
    fi
done

# Validate template files
for template in .ai-trackdown/templates/*.md; do
    if [ -f "$template" ]; then
        echo "✓ Template exists: $(basename "$template")"
    else
        echo "✗ Missing template: $(basename "$template")"
    fi
done
```

### Configuration Schema

The configuration follows this schema structure:

```yaml
# Schema validation (for reference)
schema:
  version: {type: integer, required: true}
  project: 
    type: object
    required: true
    properties:
      name: {type: string, required: true}
      id_prefix: {type: string, required: true}
      token_budget: {type: integer, minimum: 0}
      
  templates:
    type: object
    properties:
      epic: {type: string}
      issue: {type: string}
      task: {type: string}
      
  workflows:
    type: object
    properties:
      statuses: {type: array, items: {type: string}}
      labels: {type: array, items: {type: string}}
      priorities: {type: array, items: {type: string}}
```

## Configuration Examples

### Enterprise Configuration

```yaml
version: 1
project:
  name: "Enterprise Application Platform"
  description: "Large-scale enterprise platform development"
  id_prefix: "EAP"
  token_budget: 500000
  default_assignee: "@platform-lead"
  
templates:
  epic: ".ai-trackdown/templates/enterprise-epic.md"
  issue: ".ai-trackdown/templates/enterprise-issue.md"
  task: ".ai-trackdown/templates/enterprise-task.md"
  security_review: ".ai-trackdown/templates/security-review.md"
  architecture_review: ".ai-trackdown/templates/architecture-review.md"
  
workflows:
  statuses: [todo, in-progress, in-review, security-review, done, blocked, cancelled]
  labels: 
    - feature, bug, enhancement, security, documentation
    - critical, high, medium, low
    - frontend, backend, api, database, infrastructure, devops
    - team-platform, team-security, team-ui, team-data
  priorities: [low, medium, high, critical]
  
validation:
  strict_mode: true
  enforce_epic_budgets: true
  required_security_review: true
  
integrations:
  github:
    enabled: true
    config: ".ai-trackdown/integrations/github.yaml"
  jira:
    enabled: true  
    config: ".ai-trackdown/integrations/jira.yaml"
    
security:
  require_https: true
  validate_certificates: true
  mask_tokens: true
```

### Minimal Configuration

```yaml
version: 1
project:
  name: "Simple Web App"
  id_prefix: "SWA"
  token_budget: 25000
  
workflows:
  statuses: [todo, in-progress, done]
  labels: [feature, bug, enhancement]
  priorities: [low, medium, high]
```

This configuration guide provides comprehensive setup options for AI-Trackdown as a documentation framework with manual workflows and template-based management.