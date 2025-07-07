# Integration Guide

This comprehensive guide covers setting up ai-trackdown integrations with external platforms, enabling seamless bidirectional synchronization while maintaining the markdown-first approach.

## 🎯 Integration Philosophy

ai-trackdown integrations follow these principles:

1. **Local First**: Your markdown files remain the source of truth
2. **Bidirectional Sync**: Changes flow both ways automatically
3. **Conflict Resolution**: Last-write-wins with conflict preservation
4. **Field Mapping**: Flexible mapping between ai-trackdown and external fields
5. **Incremental Updates**: Only sync what's changed for efficiency

## 🐙 GitHub Issues Integration

### Overview
GitHub Issues integration provides seamless synchronization between ai-trackdown tasks and GitHub repository issues, enabling teams to maintain their existing GitHub workflows while benefiting from ai-trackdown's AI-native features.

### Setup

#### 1. Prerequisites
- GitHub repository with Issues enabled
- Personal access token or GitHub App credentials
- Repository admin or write access

#### 2. Authentication Setup

**Option A: Personal Access Token (Recommended for individuals)**
```bash
# Create token at: https://github.com/settings/tokens
# Required scopes: repo, read:org, read:user

ai-trackdown sync setup github \
  --repo="myorg/myrepo" \
  --auth=token \
  --token="${GITHUB_TOKEN}"
```

**Option B: GitHub App (Recommended for organizations)**
```bash
# Create GitHub App at: https://github.com/settings/apps
# Required permissions: Issues (read/write), Metadata (read)

ai-trackdown sync setup github \
  --repo="myorg/myrepo" \
  --auth=app \
  --app-id="${GITHUB_APP_ID}" \
  --private-key-path="./github-app.pem"
```

#### 3. Configuration
The setup creates `.ai-trackdown/sync/github.yaml`:

```yaml
# GitHub Integration Configuration
provider: github
enabled: true
repository: "myorg/myrepo"
auth_method: "token"  # or "app"

# Field mapping configuration
mapping:
  issue:
    # ai-trackdown → GitHub
    title: title
    body: |
      ${description}
      
      ## Acceptance Criteria
      ${acceptance_criteria}
      
      ## Technical Notes
      ${technical_notes}
    labels: labels
    assignee: 
      transform: strip_at  # @username → username
    milestone: epic.github_milestone
    
  # GitHub → ai-trackdown  
  reverse_mapping:
    title: title
    body: description
    labels: labels
    assignee: 
      transform: add_at  # username → @username
    milestone: epic

# Synchronization rules
sync_rules:
  # What to sync
  directions: ["push", "pull"]  # bidirectional
  include_types: ["issue", "task"]
  exclude_labels: ["no-sync", "ai-trackdown-ignore"]
  
  # When to sync
  auto_sync: true
  sync_interval: "15m"
  sync_on_commit: true
  sync_on_push: true
  
  # Conflict resolution
  conflict_strategy: "last_write_wins"
  create_conflict_files: true
  notify_conflicts: true

# GitHub-specific settings
github:
  # Issue creation
  auto_create_issues: true
  default_labels: ["ai-trackdown"]
  issue_template: |
    <!-- Created by ai-trackdown -->
    ${body}
    
    ---
    **ai-trackdown ID**: ${id}
    **Epic**: ${epic}
    **Estimate**: ${estimate} points
    
  # Webhook configuration (optional)
  webhook:
    enabled: false
    url: "https://yourserver.com/webhook/github"
    secret: "${GITHUB_WEBHOOK_SECRET}"
    events: ["issues", "issue_comment"]

# Advanced options
options:
  preserve_github_metadata: true
  sync_comments: true
  sync_events: false
  update_existing: true
  archive_closed: false
```

### Usage

#### Initial Sync
```bash
# Full bidirectional sync
ai-trackdown sync github --full

# Push local tasks to GitHub
ai-trackdown sync github --push-only

# Pull GitHub issues to local
ai-trackdown sync github --pull-only
```

#### Ongoing Sync
```bash
# Incremental sync (only changes since last sync)
ai-trackdown sync github

# Sync specific time range
ai-trackdown sync github --since="2025-01-07T10:00:00Z"

# Sync specific items
ai-trackdown sync github --items="ISSUE-001,ISSUE-002"
```

#### Status and Monitoring
```bash
# Check sync status
ai-trackdown sync status github

# View sync history
ai-trackdown sync history github --limit=10

# Resolve sync conflicts
ai-trackdown sync resolve github --interactive
```

### Field Mapping Examples

#### Complex Body Mapping
```yaml
mapping:
  issue:
    body: |
      ## Description
      ${description}
      
      ## Acceptance Criteria
      ${acceptance_criteria}
      
      ## Technical Implementation
      ${technical_notes}
      
      ## AI Context
      ${ai_context}
      
      ---
      **ai-trackdown Metadata**
      - ID: `${id}`
      - Epic: ${epic}
      - Estimate: ${estimate} story points
      - Token Usage: ${token_usage.total} tokens
```

#### Label Transformation
```yaml
mapping:
  issue:
    labels:
      transform: |
        # Convert priority labels
        labels.map(label => {
          if (label.startsWith('priority-')) {
            return `prio::${label.replace('priority-', '')}`;
          }
          return label;
        })
```

#### Custom Field Mapping
```yaml
mapping:
  issue:
    # Map to GitHub issue fields
    title: title
    body: description
    labels: 
      - transform: flatten
      - add: ["ai-trackdown", "${type}"]
    assignee: assignee.github_username
    
    # Map to GitHub project fields (beta)
    project_fields:
      Status: status
      Priority: 
        transform: priority_to_number
      Epic: epic.title
```

### Webhook Integration

For real-time synchronization, set up GitHub webhooks:

#### 1. Webhook Endpoint Setup
```bash
# Enable webhook support
ai-trackdown sync setup webhook github \
  --url="https://yourserver.com/ai-trackdown/webhook" \
  --secret="${WEBHOOK_SECRET}"
```

#### 2. GitHub Webhook Configuration
In your GitHub repository settings:
- **Payload URL**: `https://yourserver.com/ai-trackdown/webhook/github`
- **Content Type**: `application/json`
- **Secret**: Your webhook secret
- **Events**: 
  - Issues
  - Issue comments
  - Pull requests (optional)

#### 3. Webhook Handler
```javascript
// Express.js webhook handler example
app.post('/ai-trackdown/webhook/github', async (req, res) => {
  const event = req.headers['x-github-event'];
  const payload = req.body;
  
  // Verify webhook signature
  const signature = req.headers['x-hub-signature-256'];
  if (!verifySignature(payload, signature, process.env.GITHUB_WEBHOOK_SECRET)) {
    return res.status(401).send('Unauthorized');
  }
  
  // Process the event
  await ai-trackdown.webhook.process('github', event, payload);
  
  res.status(200).send('OK');
});
```

### Troubleshooting GitHub Integration

#### Common Issues

**"Repository not found"**
```bash
# Check repository access
gh repo view myorg/myrepo

# Verify token permissions
ai-trackdown sync test github --verbose
```

**"Rate limit exceeded"**
```bash
# Check rate limit status
ai-trackdown sync status github --rate-limits

# Configure rate limiting
ai-trackdown config set github.rate_limit_strategy=exponential_backoff
```

**"Sync conflicts"**
```bash
# List conflicts
ai-trackdown sync conflicts github

# Resolve conflicts interactively
ai-trackdown sync resolve github --interactive

# Resolve conflicts automatically (last-write-wins)
ai-trackdown sync resolve github --auto
```

## 🎯 Jira Integration

### Overview
Jira integration enables teams using Atlassian products to maintain their existing project management workflows while leveraging ai-trackdown's AI-native capabilities.

### Setup

#### 1. Prerequisites
- Jira Cloud or Server instance
- Admin or project admin permissions
- API token or OAuth2 credentials

#### 2. Authentication Setup

**Option A: API Token (Jira Cloud)**
```bash
# Create token at: https://id.atlassian.com/manage-profile/security/api-tokens

ai-trackdown sync setup jira \
  --url="https://yourcompany.atlassian.net" \
  --project="PROJ" \
  --auth=token \
  --username="your-email@company.com" \
  --token="${JIRA_API_TOKEN}"
```

**Option B: OAuth2 (Recommended for applications)**
```bash
# Create OAuth app in Jira
ai-trackdown sync setup jira \
  --url="https://yourcompany.atlassian.net" \
  --project="PROJ" \
  --auth=oauth2 \
  --client-id="${JIRA_CLIENT_ID}" \
  --client-secret="${JIRA_CLIENT_SECRET}"
```

#### 3. Configuration
Creates `.ai-trackdown/sync/jira.yaml`:

```yaml
# Jira Integration Configuration
provider: jira
enabled: true
url: "https://yourcompany.atlassian.net"
project: "PROJ"
auth_method: "token"

# Field mapping
mapping:
  issue:
    # ai-trackdown → Jira
    summary: title
    description: |
      h2. Description
      ${description}
      
      h2. Acceptance Criteria
      ${acceptance_criteria}
      
      h2. Technical Notes  
      ${technical_notes}
      
    issuetype:
      epic: "Epic"
      issue: "Story" 
      task: "Task"
      bug: "Bug"
      
    priority:
      priority-critical: "Highest"
      priority-high: "High"
      priority-medium: "Medium"
      priority-low: "Low"
      default: "Medium"
      
    labels: labels
    assignee: assignee.jira_username
    
    # Custom fields
    customfield_10001: estimate  # Story Points
    customfield_10002: epic.key  # Epic Link
    customfield_10003: |          # AI Context
      AI Token Usage: ${token_usage.total}
      AI Agents: ${token_usage.by_agent}

# Sync configuration
sync_rules:
  directions: ["push", "pull"]
  include_statuses: ["open", "in-progress", "in-review"]
  exclude_statuses: ["closed", "resolved"]
  
  # Status mapping
  status_mapping:
    todo: "To Do"
    in-progress: "In Progress"
    in-review: "In Review"
    done: "Done"
    blocked: "Blocked"

# Jira-specific settings
jira:
  # Issue creation
  default_issue_type: "Story"
  default_priority: "Medium"
  
  # Workflow
  use_transitions: true
  transition_mapping:
    todo -> in-progress: "Start Progress"
    in-progress -> in-review: "Create Pull Request"
    in-review -> done: "Close Issue"
    
  # Comments
  sync_comments: true
  comment_prefix: "[ai-trackdown] "
  
  # Attachments
  sync_attachments: false
```

### Advanced Jira Features

#### Epic Linking
```yaml
# Automatic epic linking
epic_handling:
  auto_create_epics: true
  epic_prefix: "EPIC-"
  epic_custom_field: "customfield_10002"
  
  # Epic template
  epic_template:
    summary: "${title}"
    description: |
      h1. ${title}
      
      h2. Overview
      ${description}
      
      h2. Success Criteria
      ${success_criteria}
      
      h2. Token Budget
      Budget: ${token_budget} tokens
      Used: ${token_usage.total} tokens
```

#### Custom Field Mapping
```yaml
# Map ai-trackdown fields to Jira custom fields
custom_fields:
  # Token tracking
  "customfield_10010":
    source: token_usage.total
    type: number
    
  "customfield_10011":  
    source: token_usage.claude
    type: number
    
  # AI context
  "customfield_10012":
    source: ai_context
    type: text
    transform: markdown_to_jira
    
  # Epic reference
  "customfield_10013":
    source: epic.jira_key
    type: epic_link
```

### Usage Examples

#### Project Setup
```bash
# Initial project sync
ai-trackdown sync jira --setup-project

# Sync all epics first
ai-trackdown sync jira --type=epic --create-missing

# Then sync issues
ai-trackdown sync jira --type=issue --link-epics
```

#### Workflow Integration
```bash
# Sync with workflow transitions
ai-trackdown sync jira --use-transitions

# Bulk status update
ai-trackdown sync jira --bulk-update-status

# Sync specific JQL query
ai-trackdown sync jira --jql="project = PROJ AND status = 'In Progress'"
```

## 📐 Linear Integration

### Overview
Linear integration provides synchronization with Linear's modern issue tracking platform, maintaining Linear's clean UX while adding ai-trackdown's AI capabilities.

### Setup

#### 1. Authentication
```bash
# Create API key at: https://linear.app/settings/api
ai-trackdown sync setup linear \
  --team="engineering" \
  --token="${LINEAR_API_KEY}"
```

#### 2. Configuration
Creates `.ai-trackdown/sync/linear.yaml`:

```yaml
# Linear Integration Configuration
provider: linear
enabled: true
team: "engineering"
auth_method: "token"

# Field mapping
mapping:
  issue:
    title: title
    description: |
      ${description}
      
      ## Acceptance Criteria
      ${acceptance_criteria}
      
      ## AI Context
      Token Usage: ${token_usage.total} tokens
      
    state:
      todo: "Todo"
      in-progress: "In Progress" 
      in-review: "In Review"
      done: "Done"
      
    priority:
      priority-critical: 1  # Urgent
      priority-high: 2      # High  
      priority-medium: 3    # Medium
      priority-low: 4       # Low
      
    assignee: assignee.linear_id
    labels: labels
    estimate: estimate

# Linear-specific features
linear:
  # Project settings
  project_id: "proj_123abc"
  
  # Cycle management
  auto_assign_cycle: true
  current_cycle_only: false
  
  # Triage
  enable_triage: true
  triage_team: "triage"
  
  # Relations
  sync_relations: true
  relation_mapping:
    blocks: "blocks"
    blocked_by: "blocked"
    relates: "related"
```

### Linear-Specific Features

#### Cycle Management
```bash
# Sync current cycle only
ai-trackdown sync linear --current-cycle

# Sync specific cycle
ai-trackdown sync linear --cycle="CYC-123"

# Create new cycle from epic
ai-trackdown linear create-cycle EPIC-001 --start-date="2025-01-15"
```

#### Project Workflows
```bash
# Sync with project milestones
ai-trackdown sync linear --projects --milestones

# Roadmap integration
ai-trackdown linear roadmap --epic=EPIC-001 --quarter=Q1-2025
```

## 🔄 Multi-Platform Sync

### Sync Orchestration
When using multiple integrations, ai-trackdown orchestrates sync operations:

```bash
# Sync all configured platforms
ai-trackdown sync --all

# Sync in specific order
ai-trackdown sync --platforms=github,jira,linear --sequential

# Parallel sync (faster but may cause conflicts)
ai-trackdown sync --platforms=github,jira --parallel
```

### Conflict Resolution
```yaml
# Global conflict resolution strategy
conflict_resolution:
  strategy: "last_write_wins"  # or "manual", "ai_assisted"
  
  # Priority order for conflicts
  platform_priority:
    - "local"      # ai-trackdown always wins
    - "github"     # GitHub second priority
    - "jira"       # Jira third
    - "linear"     # Linear last
    
  # Field-specific resolution
  field_resolution:
    title: "local"           # Local always wins for titles
    description: "newest"    # Newest change wins
    status: "platform_priority"  # Use priority order
    labels: "merge"          # Merge label lists
```

### Cross-Platform Field Mapping
```yaml
# Unified field mapping across platforms
unified_mapping:
  priority:
    github:
      critical: ["priority: critical", "severity: high"]
      high: ["priority: high"]
      medium: ["priority: medium"]
      low: ["priority: low"]
    
    jira:
      critical: "Highest"
      high: "High"
      medium: "Medium"  
      low: "Low"
      
    linear:
      critical: 1
      high: 2
      medium: 3
      low: 4
```

## 🎛️ Advanced Integration Features

### Webhook Aggregation
```bash
# Set up unified webhook endpoint
ai-trackdown sync setup webhook --unified \
  --url="https://yourserver.com/ai-trackdown/webhook" \
  --platforms=github,jira,linear
```

### Smart Sync Strategies
```yaml
# Intelligent sync configuration
smart_sync:
  # Avoid sync loops
  loop_detection: true
  max_sync_depth: 3
  
  # Bandwidth optimization
  batch_size: 50
  rate_limit_aware: true
  
  # Content-aware sync
  detect_duplicates: true
  merge_similar_items: true
  
  # AI-assisted conflict resolution
  ai_conflict_resolution:
    enabled: true
    model: "claude-3.5-sonnet"
    confidence_threshold: 0.8
```

### Integration Analytics
```bash
# Sync performance metrics
ai-trackdown sync metrics --period=week

# Integration health check
ai-trackdown sync health --all-platforms

# Sync efficiency report
ai-trackdown sync efficiency --show-optimizations
```

## 🚨 Troubleshooting

### Common Integration Issues

#### Authentication Problems
```bash
# Test authentication
ai-trackdown sync test github --auth-only
ai-trackdown sync test jira --auth-only

# Refresh tokens
ai-trackdown sync refresh-auth --all
```

#### Sync Conflicts
```bash
# Identify conflict sources
ai-trackdown sync conflicts --analyze

# Conflict resolution workflow
ai-trackdown sync resolve --interactive --platform=github
```

#### Performance Issues
```bash
# Profile sync operations
ai-trackdown sync profile --platform=jira --verbose

# Optimize sync configuration
ai-trackdown sync optimize --auto-configure
```

### Debug Mode
```bash
# Enable detailed logging
export AITASKTRACK_DEBUG=true
ai-trackdown sync github --verbose

# Trace sync operations
ai-trackdown sync github --trace --output=sync-trace.log
```

## 📋 Integration Checklist

### Pre-Integration
- [ ] Backup existing tasks (`ai-trackdown export --all`)
- [ ] Verify platform permissions
- [ ] Test authentication credentials
- [ ] Review field mapping requirements
- [ ] Plan conflict resolution strategy

### Post-Integration
- [ ] Run initial full sync (`ai-trackdown sync PLATFORM --full`)
- [ ] Verify data integrity (`ai-trackdown validate --all`)
- [ ] Test bidirectional sync with sample data
- [ ] Configure monitoring and alerts
- [ ] Document team workflows

### Ongoing Maintenance
- [ ] Monitor sync health regularly
- [ ] Review and resolve conflicts promptly
- [ ] Update field mappings as needed
- [ ] Optimize sync performance
- [ ] Keep integration credentials current

## 🎯 Best Practices

### 1. Start Small
- Begin with read-only sync to test mappings
- Use test repositories/projects first
- Gradually expand scope and features

### 2. Field Mapping Strategy
- Map essential fields first (title, description, status)
- Use transformation functions for complex mappings
- Preserve platform-specific metadata

### 3. Conflict Prevention
- Establish clear ownership rules
- Use automation to minimize manual conflicts
- Regular sync intervals to avoid drift

### 4. Performance Optimization
- Use incremental sync for regular operations
- Batch operations during off-peak hours
- Monitor API rate limits

### 5. Team Coordination
- Document integration workflows
- Train team on conflict resolution
- Establish sync monitoring procedures

With these integrations, ai-trackdown becomes the central hub for your development workflow while preserving the unique capabilities of each platform you already use.