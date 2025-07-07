# ai-trackdown Configuration Examples

This document provides configuration examples for different types of projects using ai-trackdown.

---

## Basic Configuration

### Minimal `.ai-trackdown/config.yaml`

```yaml
version: 1
project:
  name: "My Project"
  id_prefix: "PROJ"
  
sync:
  github:
    enabled: false
  jira:
    enabled: false
  linear:
    enabled: false
```

### Standard Configuration

```yaml
version: 1
project:
  name: "E-commerce Platform"
  id_prefix: "ECOM"
  default_epic_budget: 25000
  
formats:
  date: "YYYY-MM-DDTHH:mm:ssZ"
  id_pattern: "{type}-{number:03d}"
  
team:
  leads:
    - "@teamlead"
    - "@techlead"
  developers:
    - "@dev1"
    - "@dev2"
    - "@dev3"
  
workflow:
  statuses:
    - "open"
    - "in-progress"
    - "review"
    - "testing"
    - "done"
    - "blocked"
    - "cancelled"
  
  labels:
    priority:
      - "priority-critical"
      - "priority-high"
      - "priority-medium"
      - "priority-low"
    type:
      - "bug"
      - "feature"
      - "enhancement"
      - "documentation"
    component:
      - "frontend"
      - "backend"
      - "api"
      - "database"
      - "infrastructure"
    
ai:
  llms_txt:
    auto_generate: false
    include_closed: false
    context_depth: 3
    
  token_tracking:
    enabled: true
    require_agent_id: true
    cost_alerts: true
    budget_alerts_threshold: 0.8
    
  agents:
    claude:
      max_tokens_per_task: 2000
      allowed_operations: ["read", "comment", "analyze"]
    gpt4:
      max_tokens_per_task: 1500
      allowed_operations: ["read", "comment"]
    copilot:
      max_tokens_per_task: 1000
      allowed_operations: ["read"]
```

---

## Project Type Configurations

### Startup MVP Project

```yaml
version: 1
project:
  name: "SaaS MVP"
  id_prefix: "MVP"
  default_epic_budget: 15000
  type: "startup"
  
team:
  size: "small"
  founders:
    - "@founder"
  technical:
    - "@cto"
    - "@dev1"
  
workflow:
  methodology: "lean"
  sprint_length: "1 week"
  
  priorities:
    - "mvp-critical"
    - "launch-blocker"
    - "nice-to-have"
    - "post-launch"
  
  phases:
    - "discovery"
    - "mvp-development"
    - "beta-testing"
    - "launch-preparation"
    - "post-launch"

metrics:
  target_launch: "2025-10-01"
  monthly_budget: 10000
  burn_rate_alert: 0.9
  
ai:
  token_tracking:
    monthly_budget: 50000
    focus: "high-impact"
    cost_per_1k:
      claude: 0.015
      gpt4: 0.030
```

### Enterprise Integration Project

```yaml
version: 1
project:
  name: "Legacy System Integration"
  id_prefix: "LSI"
  default_epic_budget: 100000
  type: "enterprise"
  
organization:
  business_unit: "IT Operations"
  budget: 5000000
  timeline: "18 months"
  
team:
  project_manager: "@pm"
  architect: "@architect"
  team_leads:
    - "@backend-lead"
    - "@frontend-lead"
    - "@qa-lead"
    - "@devops-lead"
  developers: 12
  qa_engineers: 4
  devops: 2
  
compliance:
  frameworks:
    - "SOC2"
    - "ISO27001"
    - "GDPR"
  security_reviews: true
  audit_trail: true
  
workflow:
  methodology: "SAFe"
  sprint_length: "2 weeks"
  release_cycle: "quarterly"
  
  governance:
    architecture_review: true
    security_review: true
    performance_review: true
    
  change_management:
    approval_required: true
    stakeholder_signoff: true
    
sync:
  jira:
    enabled: true
    project: "LSI"
    required: true
  confluence:
    enabled: true
    space: "INTEGRATION"
    
ai:
  token_tracking:
    monthly_budget: 200000
    require_justification: true
    approval_threshold: 10000
    
  restrictions:
    no_pii: true
    no_sensitive_data: true
    audit_all_usage: true
```

### Open Source Library

```yaml
version: 1
project:
  name: "React Component Library"
  id_prefix: "RCL"
  default_epic_budget: 20000
  type: "open-source"
  
community:
  license: "MIT"
  maintainers:
    - "@maintainer1"
    - "@maintainer2"
  contributors: "community"
  
  governance:
    rfc_process: true
    community_voting: true
    code_of_conduct: true
    
workflow:
  contribution_model: "fork-and-pr"
  review_required: true
  ci_required: true
  
  release:
    semantic_versioning: true
    changelog: true
    migration_guides: true
    
sync:
  github:
    enabled: true
    repo: "org/react-components"
    issues: true
    projects: true
    
documentation:
  storybook: true
  api_docs: true
  examples: true
  getting_started: true
  
ai:
  token_tracking:
    community_budget: 25000
    transparency: true
    public_usage_stats: true
```

### Research & Development Project

```yaml
version: 1
project:
  name: "AI Research Platform"
  id_prefix: "ARP"
  default_epic_budget: 50000
  type: "research"
  
research:
  methodology: "design-thinking"
  hypothesis_driven: true
  experiment_tracking: true
  
  phases:
    - "literature-review"
    - "hypothesis-formation"
    - "prototype-development"
    - "experimentation"
    - "validation"
    - "documentation"
    
team:
  researchers:
    - "@researcher1"
    - "@researcher2"
  engineers:
    - "@ml-engineer"
    - "@data-engineer"
  advisors:
    - "@academic-advisor"
    
workflow:
  peer_review: true
  reproducibility: true
  version_control: "everything"
  
  experiments:
    track_all: true
    document_failures: true
    share_results: true
    
ai:
  token_tracking:
    research_budget: 100000
    experiment_tracking: true
    hypothesis_testing: true
    
  usage_categories:
    - "literature-analysis"
    - "hypothesis-generation"
    - "code-analysis"
    - "result-interpretation"
    - "documentation"
```

---

## Integration Configurations

### GitHub Issues Integration

```yaml
sync:
  github:
    enabled: true
    repo: "organization/repository"
    auth_method: "token"  # or "app"
    
    mapping:
      issue:
        title: "title"
        body: |
          {{ description }}
          
          ## Acceptance Criteria
          {{ acceptance_criteria }}
          
          ## AI Context
          {{ ai_context }}
        labels: "labels"
        assignees: 
          - transform: "strip_at(assignee)"
        milestone: "epic.github_milestone"
        
    sync_rules:
      - direction: "bidirectional"
        filter: "type in ['issue', 'task']"
        exclude_labels: ['no-sync', 'internal-only']
        
    webhook:
      enabled: true
      secret: "${GITHUB_WEBHOOK_SECRET}"
      events: ['issues', 'issue_comment']
```

### Jira Integration

```yaml
sync:
  jira:
    enabled: true
    url: "https://company.atlassian.net"
    project: "PROJ"
    auth_method: "oauth2"
    
    mapping:
      issue:
        summary: "title"
        description: "description"
        issuetype:
          epic: "Epic"
          issue: "Story"
          task: "Task"
        customfield_10001: "estimate"  # Story points
        labels: "labels"
        priority:
          critical: "Highest"
          high: "High"
          medium: "Medium"
          low: "Low"
          
    field_mapping:
      epic_link: "epic"
      sprint: "labels.sprint"
      team: "labels.team"
      
    webhook:
      url: "https://yourapp.com/ai-trackdown/webhook/jira"
      secret: "${JIRA_WEBHOOK_SECRET}"
      events: ['issue_created', 'issue_updated']
```

### Linear Integration

```yaml
sync:
  linear:
    enabled: true
    team_id: "engineering"
    auth_method: "api_key"
    
    mapping:
      issue:
        title: "title"
        description: "description"
        priority:
          critical: 1
          high: 2
          medium: 3
          low: 4
        labels: "labels"
        assignee: "assignee"
        estimate: "estimate"
        
    sync_rules:
      - direction: "push_only"  # Linear as secondary
        filter: "status != 'done'"
        create_only: true
```

---

## AI Configuration Examples

### Token Budget Management

```yaml
ai:
  token_tracking:
    enabled: true
    
    providers:
      - name: "claude"
        model: "claude-3.5-sonnet"
        cost_per_1k_input: 0.003
        cost_per_1k_output: 0.015
        
      - name: "gpt4"
        model: "gpt-4-turbo"
        cost_per_1k_input: 0.01
        cost_per_1k_output: 0.03
        
      - name: "gpt4o"
        model: "gpt-4o"
        cost_per_1k_input: 0.005
        cost_per_1k_output: 0.015
        
    budgets:
      project_monthly: 1000000  # 1M tokens
      epic_default: 50000
      issue_default: 10000
      task_default: 2000
      
    alerts:
      epic_threshold: 0.8    # 80% of budget
      project_threshold: 0.9  # 90% of budget
      weekly_report: true
      
    attribution:
      method: "git-author"
      fallback: "explicit-declaration"
      require_purpose: true
```

### Agent Restrictions

```yaml
ai:
  agents:
    claude:
      max_tokens_per_task: 5000
      max_tokens_per_day: 20000
      allowed_operations:
        - "read"
        - "analyze"
        - "comment"
        - "suggest"
      restricted_operations:
        - "modify_code"
        - "delete_tasks"
      allowed_file_types:
        - ".md"
        - ".yaml"
        - ".json"
        
    gpt4:
      max_tokens_per_task: 3000
      max_tokens_per_day: 15000
      allowed_operations:
        - "read"
        - "analyze"
      requires_approval:
        - "code_generation"
        - "architecture_changes"
        
    copilot:
      max_tokens_per_task: 1000
      max_tokens_per_day: 5000
      allowed_operations:
        - "read"
      context_only: true
```

---

## Environment-Specific Configurations

### Development Environment

```yaml
version: 1
project:
  name: "My Project (Development)"
  id_prefix: "DEV"
  environment: "development"
  
ai:
  token_tracking:
    enabled: true
    debug_mode: true
    detailed_logging: true
    
  agents:
    claude:
      verbose_logging: true
      include_debug_context: true
      
sync:
  external_systems: false  # No external sync in dev
  
validation:
  strict_mode: false
  warnings_as_errors: false
```

### Production Environment

```yaml
version: 1
project:
  name: "My Project"
  id_prefix: "PROJ"
  environment: "production"
  
ai:
  token_tracking:
    enabled: true
    audit_trail: true
    cost_monitoring: true
    
  security:
    no_pii: true
    audit_all_interactions: true
    require_approval: true
    
sync:
  external_systems: true
  required_sync: ["jira"]
  
validation:
  strict_mode: true
  require_reviews: true
  
monitoring:
  metrics: true
  alerts: true
  uptime_tracking: true
```

---

## Advanced Configuration Patterns

### Multi-Team Configuration

```yaml
version: 1
project:
  name: "Multi-Team Platform"
  id_prefix: "MTP"
  
teams:
  frontend:
    lead: "@frontend-lead"
    members: ["@fe-dev1", "@fe-dev2"]
    prefix: "FE"
    budget: 25000
    
  backend:
    lead: "@backend-lead"
    members: ["@be-dev1", "@be-dev2", "@be-dev3"]
    prefix: "BE"
    budget: 35000
    
  devops:
    lead: "@devops-lead"
    members: ["@devops1"]
    prefix: "OPS"
    budget: 15000
    
workflow:
  cross_team_dependencies: true
  team_boundaries: true
  
  routing:
    frontend_issues: "frontend"
    backend_issues: "backend"
    infrastructure_issues: "devops"
```

### Compliance Configuration

```yaml
version: 1
project:
  name: "Compliance Project"
  id_prefix: "COMP"
  
compliance:
  frameworks:
    - name: "SOC2"
      controls: ["CC6.1", "CC6.7", "CC7.1"]
      audit_trail: true
      
    - name: "PCI-DSS"
      requirements: ["3.4", "8.2", "10.2"]
      documentation: required
      
  data_classification:
    levels: ["public", "internal", "confidential", "restricted"]
    default: "internal"
    
  approval_workflows:
    security_review:
      required_for: ["data-access", "encryption", "authentication"]
      approvers: ["@security-lead"]
      
    compliance_review:
      required_for: ["audit-trail", "data-retention"]
      approvers: ["@compliance-officer"]
```

### Performance Monitoring Configuration

```yaml
version: 1
project:
  name: "Performance Optimized Project"
  id_prefix: "PERF"
  
performance:
  monitoring:
    enabled: true
    metrics:
      - "task_completion_time"
      - "epic_cycle_time"
      - "token_usage_efficiency"
      - "ai_response_time"
      
  targets:
    task_completion: "3 days average"
    epic_completion: "30 days average"
    token_efficiency: "<500 tokens per task"
    
  alerts:
    slow_tasks: "7 days without update"
    token_overrun: "150% of estimate"
    blocked_items: "24 hours in blocked status"
    
ai:
  performance_optimization:
    context_caching: true
    batch_operations: true
    smart_summarization: true
    
  efficiency_tracking:
    measure_value: true
    track_outcomes: true
    optimize_prompts: true
```

---

These configuration examples provide templates for various project types and integration scenarios. Customize them based on your specific needs, team structure, and organizational requirements.
