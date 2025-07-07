# Configuration Guide

ai-trackdown uses a YAML-based configuration system that allows you to customize behavior, integrations, and AI features.

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
```

This minimal configuration sets up ai-trackdown with:
- Project name for identification
- ID prefix for generated task IDs (PROJ-0001, PROJ-0002, etc.)

### Standard Configuration

```yaml
version: 1
project:
  name: "My Web Application"
  id_prefix: "WEB"
  default_epic_budget: 50000
  
ai:
  llms_txt:
    auto_generate: true
    include_closed: false
    context_depth: 3
    
  token_tracking:
    enabled: true
    require_agent_id: true
```

## Complete Configuration Reference

### Project Settings

```yaml
project:
  name: "Project Name"              # Display name for the project
  id_prefix: "PROJ"                 # Prefix for generated IDs
  default_epic_budget: 50000        # Default token budget for new epics
  timezone: "UTC"                   # Timezone for dates (optional)
  language: "en"                    # Language for generated content (optional)
```

### Format Settings

```yaml
formats:
  date: "YYYY-MM-DDTHH:mm:ssZ"     # Date format for timestamps
  id_pattern: "{type}-{number:04d}" # ID generation pattern
  branch_pattern: "{type}/{id}-{title}" # Git branch naming pattern
```

**ID Pattern Variables:**
- `{type}` - Task type (EPIC, ISSUE, TASK)
- `{number}` - Sequential number
- `{title}` - Slugified title
- `{prefix}` - Project prefix

### AI Configuration

```yaml
ai:
  llms_txt:
    auto_generate: true             # Auto-generate llms.txt on changes
    include_closed: false           # Include closed items in llms.txt
    context_depth: 3               # Levels of related context to include
    max_items: 100                 # Maximum items in index
    update_frequency: "on_change"   # When to regenerate (on_change, hourly, daily)
    
  token_tracking:
    enabled: true                   # Enable token usage tracking
    require_agent_id: true          # Require agent identification
    cost_alerts: true              # Enable cost threshold alerts
    detailed_logging: false         # Log detailed token usage
    
  agents:
    claude:
      max_tokens_per_task: 5000     # Maximum tokens per task
      allowed_operations:           # Permitted operations
        - "read"
        - "comment"
        - "update"
      cost_limit_daily: 10.00      # Daily cost limit in USD
      
    gpt4:
      max_tokens_per_task: 3000
      allowed_operations: ["read", "comment"]
      cost_limit_daily: 5.00
```

### Token Tracking Configuration

```yaml
token_tracking:
  providers:
    - name: claude
      model: claude-3.5-sonnet
      cost_per_1k_input: 0.003
      cost_per_1k_output: 0.015
      
    - name: gpt4
      model: gpt-4-turbo
      cost_per_1k_input: 0.01
      cost_per_1k_output: 0.03
      
    - name: copilot
      model: github-copilot
      cost_per_1k_input: 0.002
      cost_per_1k_output: 0.004
      
  attribution:
    method: git-author              # How to attribute tokens (git-author, explicit-declaration)
    fallback: explicit-declaration  # Fallback method
    auto_detect_agents: true        # Try to detect AI agents from commit messages
    
  budgets:
    project_monthly: 1000000        # Monthly project token budget
    epic_default: 50000            # Default epic token budget
    task_default: 5000             # Default task token budget
    alert_threshold: 0.8           # Alert when % of budget used
    
  reports:
    frequency: weekly              # Report generation frequency
    recipients: ["team@company.com"] # Email recipients for reports
    include_costs: true            # Include cost information
```

### Git Integration

```yaml
git:
  hooks:
    auto_install: true             # Automatically install git hooks
    commit_parsing: true           # Parse commit messages for task updates
    branch_protection: false       # Prevent commits without task references
    
  commit_patterns:
    - pattern: "^(feat|fix|docs)\\(([A-Z]+-\\d+)\\):"
      task_group: 2                # Regex group containing task ID
      
  branch_naming:
    enforce: false                 # Enforce branch naming convention
    pattern: "{type}/{id}-{title}" # Branch naming pattern
    
  auto_status:
    fix: "done"                   # Status to set when commit contains "fix"
    close: "done"                 # Status to set when commit contains "close"
    resolve: "done"               # Status to set when commit contains "resolve"
    start: "in-progress"          # Status to set when commit contains "start"
```

## Integration Configurations

### GitHub Issues

```yaml
sync:
  github:
    enabled: true
    repo: "org/repository"         # GitHub repository
    auth_method: "token"           # Authentication method (token, app)
    
    # For personal access token
    token: "${GITHUB_TOKEN}"       # Environment variable reference
    
    # For GitHub App
    app_id: "${GITHUB_APP_ID}"
    private_key_path: "path/to/key.pem"
    installation_id: "${GITHUB_INSTALLATION_ID}"
    
    sync_frequency: "hourly"       # How often to sync
    direction: "bidirectional"     # Sync direction (push, pull, bidirectional)
    
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
        milestone: epic.github_milestone_number
        
    webhook:
      enabled: true
      secret: "${GITHUB_WEBHOOK_SECRET}"
      endpoint: "/webhook/github"
```

### Jira Integration

```yaml
sync:
  jira:
    enabled: true
    url: "https://company.atlassian.net"
    project: "PROJ"               # Jira project key
    auth_method: "basic"          # Authentication method (basic, oauth2, token)
    
    # For basic auth
    username: "${JIRA_USERNAME}"
    password: "${JIRA_PASSWORD}"
    
    # For OAuth2
    client_id: "${JIRA_CLIENT_ID}"
    client_secret: "${JIRA_CLIENT_SECRET}"
    
    mapping:
      epic:
        summary: title
        description: description
        issuetype: "Epic"
        
      issue:
        summary: title
        description: description
        issuetype: "Story"
        customfield_10001: estimate  # Story points field
        
    field_mappings:
      priority:
        priority-high: "High"
        priority-medium: "Medium"
        priority-low: "Low"
        
    webhook:
      url: "https://your-domain.com/ai-trackdown/webhook/jira"
      secret: "${JIRA_WEBHOOK_SECRET}"
```

### Linear Integration

```yaml
sync:
  linear:
    enabled: true
    api_key: "${LINEAR_API_KEY}"
    team_id: "engineering"        # Linear team identifier
    
    mapping:
      issue:
        title: title
        description: description
        priority: 
          priority-high: 1
          priority-medium: 2
          priority-low: 3
          
    sync_labels: true            # Sync labels between systems
    create_missing_labels: true  # Create labels that don't exist
```

## Environment Variables

ai-trackdown supports environment variables for sensitive configuration:

```bash
# GitHub
export GITHUB_TOKEN="ghp_xxxxxxxxxxxx"
export GITHUB_WEBHOOK_SECRET="your-webhook-secret"

# Jira
export JIRA_USERNAME="your-username"
export JIRA_PASSWORD="your-password"
export JIRA_WEBHOOK_SECRET="your-webhook-secret"

# Linear
export LINEAR_API_KEY="lin_api_xxxxxxxxxxxx"

# AI Tokens (if using external APIs)
export ANTHROPIC_API_KEY="sk-xxxxxxxxxxxx"
export OPENAI_API_KEY="sk-xxxxxxxxxxxx"
```

### Environment Variable References

In config files, reference environment variables using:
```yaml
token: "${GITHUB_TOKEN}"           # Required variable
token: "${GITHUB_TOKEN:-default}"  # With default value
```

## Configuration Validation

### Validate Configuration

```bash
ai-trackdown config validate
```

### Check Integration Setup

```bash
ai-trackdown config test --integration=github
ai-trackdown config test --integration=jira
ai-trackdown config test --all
```

### Configuration Templates

Generate configuration templates:

```bash
# Basic template
ai-trackdown config init

# With GitHub integration
ai-trackdown config init --github

# With all integrations
ai-trackdown config init --full
```

## Advanced Configuration

### Custom Field Mappings

```yaml
custom_fields:
  github:
    custom_field_1: "github_field_name"
    custom_field_2: "another_field"
    
  jira:
    custom_field_1: "customfield_10001"
    custom_field_2: "customfield_10002"
```

### Conditional Sync Rules

```yaml
sync_rules:
  - name: "Critical Issues Only"
    condition: "labels contains 'critical'"
    integrations: ["github", "jira"]
    
  - name: "Development Team Tasks"
    condition: "assignee in ['@dev1', '@dev2']"
    integrations: ["linear"]
    
  - name: "Exclude Documentation"
    condition: "NOT labels contains 'docs'"
    integrations: ["all"]
```

### Custom Workflows

```yaml
workflows:
  issue_lifecycle:
    states:
      - name: "open"
        transitions: ["in-progress", "blocked"]
      - name: "in-progress"
        transitions: ["in-review", "blocked", "open"]
      - name: "in-review"
        transitions: ["done", "in-progress"]
      - name: "blocked"
        transitions: ["open", "in-progress"]
      - name: "done"
        transitions: []
        
  auto_transitions:
    - trigger: "commit_contains_fix"
      from: "in-progress"
      to: "done"
```

## Configuration Migration

### Upgrading Configuration

When upgrading ai-trackdown versions:

```bash
# Backup current config
ai-trackdown config backup

# Migrate to new format
ai-trackdown config migrate

# Validate migrated config
ai-trackdown config validate
```

### Version Compatibility

| Config Version | ai-trackdown Version | Migration Required |
|---------------|---------------------|-------------------|
| 1.0 | 1.0.x | No |
| 1.1 | 1.1.x | Automatic |
| 2.0 | 2.0.x | Manual |

## Troubleshooting

### Common Issues

**Configuration not found:**
```bash
# Initialize configuration
ai-trackdown init
```

**Invalid YAML syntax:**
```bash
# Validate YAML
ai-trackdown config validate --syntax-only
```

**Environment variables not resolved:**
```bash
# Check environment
ai-trackdown config env-check
```

**Integration authentication fails:**
```bash
# Test specific integration
ai-trackdown config test --integration=github --verbose
```

### Debug Configuration

```bash
# Show effective configuration
ai-trackdown config show

# Show with resolved environment variables
ai-trackdown config show --resolved

# Export configuration
ai-trackdown config export > config-backup.yaml
```

---

For more help with configuration, see:
- [Integration Setup Guide](integrations.md)
- [Token Tracking Guide](token-tracking.md)
- [CLI Reference](cli-reference.md)