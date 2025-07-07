# ai-trackdown: Design Document
## Lightweight AI-Native Task Management System

**Version:** 1.0  
**Status:** Draft  
**Authors:** System Architecture Team  
**Last Updated:** January 2025

---

## Executive Summary

ai-trackdown is a lightweight, git-native task management system designed from the ground up to support both human developers and AI agents. It combines the simplicity of markdown-based task tracking with native support for the llms.txt standard, comprehensive token tracking, and bidirectional synchronization with major task management platforms.

### Key Features
- Pure markdown/text storage using GitHub Flavored Markdown (GFM)
- Native llms.txt standard implementation for AI discoverability
- Git-native with automatic state management via hooks
- Token usage tracking at task/epic/project levels
- Bidirectional sync with GitHub Issues, Jira, Linear
- Zero external dependencies for core functionality

---

## System Architecture

### Core Design Principles

1. **Text-First**: All data stored as human-readable markdown files
2. **Git-Native**: Leverages git's distributed nature and version control
3. **AI-Optimized**: Structured for minimal token consumption
4. **Tool-Agnostic**: Works with any text editor or IDE
5. **Progressive Enhancement**: Basic features work offline, advanced features when connected

### Directory Structure

```
project-root/
├── .ai-trackdown/
│   ├── config.yaml              # System configuration
│   ├── sync/                    # Sync state and mappings
│   │   ├── github.json
│   │   ├── jira.json
│   │   └── linear.json
│   └── llms.txt                 # Auto-generated index
├── tasks/
│   ├── epics/
│   │   ├── EPIC-001-user-authentication.md
│   │   └── EPIC-002-payment-integration.md
│   ├── issues/
│   │   ├── ISSUE-001-login-flow.md
│   │   └── ISSUE-002-password-reset.md
│   └── tasks/
│       ├── TASK-001-create-login-form.md
│       └── TASK-002-implement-oauth.md
├── docs/
│   └── llms-full.txt            # Complete task context
└── TASKTRACK.md                 # Project task overview
```

---

## File Formats

### Task File Structure (GFM + Extensions)

```markdown
---
id: ISSUE-001
type: issue
title: Implement user login flow
status: in-progress
epic: EPIC-001
assignee: @johndoe
created: 2025-01-07T10:00:00Z
updated: 2025-01-07T14:30:00Z
labels: [authentication, frontend, priority-high]
estimate: 5
token_usage:
  total: 1247
  by_agent:
    claude: 892
    copilot: 355
sync:
  github: 145
  jira: AUTH-234
---

# Implement user login flow

## Description
Create a secure login flow with email/password authentication and OAuth2 support.

## Acceptance Criteria
- [ ] Email/password login form with validation
- [ ] OAuth2 integration (Google, GitHub)
- [ ] Remember me functionality
- [ ] Rate limiting on login attempts
- [x] Session management

## Technical Notes
- Use JWT for session tokens
- Implement PKCE flow for OAuth2
- Store sessions in Redis

## Token Context
<!-- AI_CONTEXT_START -->
This issue implements the authentication flow for the user management system. 
Related files: `src/auth/*`, `src/components/LoginForm.tsx`
Dependencies: Redis for session storage, JWT library
<!-- AI_CONTEXT_END -->

## Activity Log
- 2025-01-07T10:00:00Z: Created by @johndoe
- 2025-01-07T11:30:00Z: Claude analyzed requirements (tokens: 347)
- 2025-01-07T14:30:00Z: Updated by @ai-agent-claude (tokens: 545)

## Related Tasks
- Blocks: TASK-003, TASK-004
- Blocked by: ISSUE-002
- Related: ISSUE-005
```

### Epic File Structure

```markdown
---
id: EPIC-001
type: epic
title: User Authentication System
status: in-progress
owner: @teamlead
created: 2025-01-01T09:00:00Z
target_date: 2025-02-15T00:00:00Z
token_budget: 50000
token_usage:
  total: 12847
  remaining: 37153
---

# User Authentication System

## Overview
Complete authentication and authorization system supporting multiple auth methods.

## Success Metrics
- Support 100k concurrent users
- < 200ms auth response time
- 99.9% uptime

## Issues
- [x] ISSUE-001: Login flow
- [ ] ISSUE-002: Password reset
- [ ] ISSUE-003: Two-factor authentication
- [ ] ISSUE-004: Role-based permissions

## Token Tracking
| Date | Agent | Task | Tokens | Purpose |
|------|-------|------|--------|---------|
| 2025-01-07 | Claude | ISSUE-001 | 892 | Requirements analysis |
| 2025-01-07 | Copilot | TASK-001 | 355 | Code generation |
```

---

## LLMs.txt Implementation

### `/llms.txt` (Index File)
```txt
# ai-trackdown Project Index
# Generated: 2025-01-07T15:00:00Z
# Format: llms.txt v1.0

## Project Overview
Task management for Project Alpha
Active epics: 3
Open issues: 27
Completed tasks: 145

## Key Files
/tasks/epics/: High-level project epics
/tasks/issues/: Active development issues
/tasks/tasks/: Granular implementation tasks
/TASKTRACK.md: Project status dashboard

## Current Focus
- EPIC-001: User Authentication (70% complete)
- ISSUE-001: Login flow (in-progress)
- ISSUE-007: API rate limiting (blocked)

## Quick Commands
View all open issues: `ai-trackdown list --status=open`
Check token usage: `ai-trackdown tokens --this-week`
Sync with GitHub: `ai-trackdown sync github`

## Integration Status
GitHub: Synced (2025-01-07T14:55:00Z)
Jira: Synced (2025-01-07T14:00:00Z)
Linear: Pending
```

### `/docs/llms-full.txt` (Complete Context)
```txt
# ai-trackdown Complete Context
# Generated: 2025-01-07T15:00:00Z

## Active Work Items

### EPIC-001: User Authentication System
Status: in-progress (70% complete)
Token Budget: 50,000 (25.7% used)

Current focus: Implementing OAuth2 integration
Blockers: Waiting for security review on session management

Issues:
- ISSUE-001: Login flow (in-progress)
  - Assignee: @johndoe
  - 3/5 criteria complete
  - Next: OAuth2 implementation
  
[... comprehensive task details ...]

## Code Context
Primary authentication logic: src/auth/
Frontend components: src/components/auth/
Tests: tests/auth/

## Recent Decisions
- 2025-01-06: Chose JWT over sessions for stateless auth
- 2025-01-05: Selected Redis for session storage
```

---

## Git Integration

### Git Hooks

#### `post-commit`
```bash
#!/bin/bash
# Auto-update task status based on commit messages

# Parse commit message for task references
# Format: "fix(ISSUE-001): Implement login form"
# Updates status based on keywords: fix/close/resolve -> done

ai-trackdown parse-commit "$1"
ai-trackdown generate-llms-txt
```

#### `pre-push`
```bash
#!/bin/bash
# Sync with external systems before push

ai-trackdown sync --all
ai-trackdown validate-tokens --warn-threshold=1000
```

### Branch Naming Convention
```
feature/ISSUE-001-login-flow
bugfix/TASK-042-fix-validation
epic/EPIC-003-payment-system
```

---

## Bidirectional Sync Architecture

### Sync Strategy

1. **Local First**: Local markdown files are source of truth
2. **Conflict Resolution**: Last-write-wins with conflict files
3. **Field Mapping**: Configurable field mappings per platform
4. **Incremental Sync**: Only sync changed items via timestamps

### GitHub Issues Integration

```yaml
# .ai-trackdown/sync/github.yaml
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
  
sync_rules:
  - direction: bidirectional
    filter: "type in ['issue', 'task']"
    exclude_labels: ['no-sync']
```

### Jira Integration

```yaml
# .ai-trackdown/sync/jira.yaml
mapping:
  issue:
    summary: title
    description: description
    issuetype: 
      issue: Story
      task: Task
      epic: Epic
    customfield_10001: estimate  # Story points
    labels: labels
    
webhook:
  url: https://your-domain.com/ai-trackdown/webhook/jira
  secret: ${JIRA_WEBHOOK_SECRET}
```

---

## Token Tracking System

### Token Attribution

```yaml
# Token usage configuration
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
      
  attribution:
    method: git-author
    fallback: explicit-declaration
    
  budgets:
    project_monthly: 1000000
    epic_default: 50000
    alert_threshold: 0.8
```

### Token Reports

```bash
# Weekly token usage report
$ ai-trackdown tokens report --period=week

Token Usage Report (2025-01-01 to 2025-01-07)
============================================
Total Tokens: 47,293
Total Cost: $1.42

By Epic:
  EPIC-001 (Auth): 12,847 tokens ($0.39) - 25.7% of budget
  EPIC-002 (API):   8,234 tokens ($0.25) - 16.5% of budget

By Agent:
  Claude:     28,456 tokens ($0.85)
  GPT-4:      12,837 tokens ($0.39)
  Copilot:     6,000 tokens ($0.18)

Top Token Consumers:
  1. ISSUE-001: 3,892 tokens (requirements analysis)
  2. ISSUE-007: 2,847 tokens (architecture review)
```

---

## CLI Interface

### Core Commands

```bash
# Initialize new project
ai-trackdown init

# Create items
ai-trackdown create epic "Payment Integration"
ai-trackdown create issue "Implement Stripe webhook" --epic=EPIC-002
ai-trackdown create task "Add webhook endpoint" --issue=ISSUE-010

# List and filter
ai-trackdown list --type=issue --status=open --assignee=@me
ai-trackdown list --epic=EPIC-001 --has-ai-context

# Update status
ai-trackdown update ISSUE-001 --status=in-review
ai-trackdown close TASK-042 --comment="Fixed in commit abc123"

# Token tracking
ai-trackdown tokens add ISSUE-001 --agent=claude --count=892
ai-trackdown tokens report --by=epic --period=month

# Sync operations
ai-trackdown sync github --full
ai-trackdown sync jira --since=1h
ai-trackdown sync status

# AI operations
ai-trackdown generate llms-txt
ai-trackdown analyze ISSUE-001 --suggest-tasks
ai-trackdown context EPIC-001 --for=claude
```

---

## Implementation Roadmap

### Phase 1: Core System (Weeks 1-4)
- [ ] Basic file structure and CLI
- [ ] Git hooks implementation  
- [ ] Markdown parsing and validation
- [ ] Simple status management

### Phase 2: AI Integration (Weeks 5-6)
- [ ] llms.txt generation
- [ ] Token tracking system
- [ ] AI context markers
- [ ] Claude/GPT optimization

### Phase 3: Sync Capabilities (Weeks 7-10)
- [ ] GitHub Issues bidirectional sync
- [ ] Jira integration with webhooks
- [ ] Linear API integration
- [ ] Conflict resolution system

### Phase 4: Advanced Features (Weeks 11-12)
- [ ] Web UI (optional)
- [ ] Analytics dashboard
- [ ] Multi-project support
- [ ] Plugin system

---

## Security Considerations

### API Token Management
- Tokens stored in OS keychain/credential manager
- Never committed to repository
- Separate tokens per integration
- Automatic token rotation support

### Audit Trail
```markdown
## Audit Log Format
2025-01-07T14:30:00Z | @ai-agent-claude | UPDATE | ISSUE-001 | status: open->in-progress | tokens: 234
2025-01-07T14:31:00Z | @johndoe | COMMENT | ISSUE-001 | "Approved for implementation"
```

### Access Control
- Git-based permissions (read/write access)
- Integration-specific permissions
- AI agent restrictions via config

---

## Example Configurations

### Minimal `.ai-trackdown/config.yaml`
```yaml
version: 1
project:
  name: "My Project"
  id_prefix: "PROJ"
  
sync:
  github:
    enabled: true
    repo: "org/repo"
```

### Full Configuration
```yaml
version: 1
project:
  name: "Enterprise Project"
  id_prefix: "ENT"
  default_epic_budget: 100000
  
formats:
  date: "YYYY-MM-DDTHH:mm:ssZ"
  id_pattern: "{type}-{number:04d}"
  
sync:
  github:
    enabled: true
    repo: "enterprise/main-app"
    auth_method: "app"
    app_id: ${GITHUB_APP_ID}
    
  jira:
    enabled: true
    url: "https://enterprise.atlassian.net"
    project: "MAIN"
    auth_method: "oauth2"
    
  linear:
    enabled: true
    team_id: "engineering"
    
ai:
  llms_txt:
    auto_generate: true
    include_closed: false
    context_depth: 3
    
  token_tracking:
    enabled: true
    require_agent_id: true
    cost_alerts: true
    
  agents:
    claude:
      max_tokens_per_task: 5000
      allowed_operations: ["read", "comment", "update"]
    
git:
  hooks:
    auto_install: true
    commit_parsing: true
    branch_protection: true
```

---

## Success Metrics

### Adoption Indicators
- Zero-friction onboarding (< 5 min setup)
- Works with existing git workflows
- No breaking changes to current tools

### Performance Targets
- < 100ms for any read operation
- < 500ms for sync operations
- < 1s for llms.txt generation
- Supports 10k+ tasks per repository

### AI Efficiency
- 50% reduction in token usage via context optimization
- 90% reduction in repeated context explanations
- Accurate token cost predictions (± 5%)

---

## Conclusion

ai-trackdown bridges the gap between traditional task management and AI-native development workflows. By leveraging git's distributed nature, markdown's simplicity, and modern AI standards, it provides a future-proof foundation for human-AI collaborative development while maintaining compatibility with existing tools and workflows.