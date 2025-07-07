# ai-trackdown CLI Usage Patterns

**Version:** 1.0  
**Status:** Specification  
**Last Updated:** January 2025

## Table of Contents

1. [Daily Developer Workflows](#daily-developer-workflows)
2. [AI Agent Interaction Patterns](#ai-agent-interaction-patterns)
3. [Team Collaboration Scenarios](#team-collaboration-scenarios)
4. [Token Management Workflows](#token-management-workflows)
5. [Integration Sync Patterns](#integration-sync-patterns)
6. [Advanced Usage Scenarios](#advanced-usage-scenarios)

---

## Daily Developer Workflows

### Morning Standup Preparation

**Scenario:** Developer preparing for daily standup meeting

```bash
# Check what I worked on yesterday
ai-trackdown list --assignee=@me --updated-after=yesterday

# Show my current active tasks
ai-trackdown list --assignee=@me --status=in-progress

# Quick status overview
ai-trackdown show ISSUE-042 --format=json | jq '.status,.title'

# Check if any tasks are blocked
ai-trackdown list --assignee=@me --status=blocked
```

**Expected Output:**
```
Tasks updated yesterday:
ISSUE-042  issue  Fix authentication bug    done       @johndoe  auth,bugfix
TASK-031   task   Update login form         in-review  @johndoe  frontend

Currently in progress:
ISSUE-045  issue  Implement rate limiting   in-progress @johndoe  security,api

No blocked tasks found.
```

### Starting New Work

**Scenario:** Developer beginning work on a new task

```bash
# Find available tasks assigned to me
ai-trackdown list --assignee=@me --status=open

# Get detailed context for a task
ai-trackdown show ISSUE-045 --include-related --include-tokens

# Check if there are any dependencies
ai-trackdown show ISSUE-045 | grep -A5 "Related Tasks"

# Update status to indicate work started
ai-trackdown update ISSUE-045 --status=in-progress --comment="Starting implementation"

# Generate AI context for coding assistance
ai-trackdown context ISSUE-045 --for=claude --include-related
```

### Commit and Task Updates

**Scenario:** Developer completing work and updating task status

```bash
# Commit with task reference (auto-updates via git hook)
git commit -m "feat(ISSUE-045): Add rate limiting middleware

- Implement token bucket algorithm
- Add configuration options
- Include comprehensive tests"

# Manual status update if needed
ai-trackdown update ISSUE-045 --status=in-review --comment="Ready for code review"

# Record token usage from AI assistance
ai-trackdown tokens add ISSUE-045 --agent=claude --count=1250 --purpose="Code review and optimization"

# Check if epic budget is still healthy
ai-trackdown tokens budget check EPIC-003
```

### End of Day Wrap-up

**Scenario:** Developer wrapping up work for the day

```bash
# Update status of current work
ai-trackdown update ISSUE-045 --status=in-progress --comment="Completed middleware implementation, working on tests tomorrow"

# Check token usage for the day
ai-trackdown tokens report --period=today --by=task

# Sync with external platforms
ai-trackdown sync all

# Preview what I'll work on tomorrow
ai-trackdown list --assignee=@me --status=open --sort=priority --reverse
```

---

## AI Agent Interaction Patterns

### Claude Assistant Task Analysis

**Scenario:** Claude analyzing a complex task and suggesting breakdown

```bash
# Claude gets task context
ai-trackdown context EPIC-003 --for=claude --include-related --max-tokens=4000

# Claude analyzes and suggests subtasks
ai-trackdown analyze EPIC-003 --suggest-tasks

# Claude records its token usage
ai-trackdown tokens add EPIC-003 --agent=claude --count=3247 --purpose="Epic analysis and task breakdown"

# Claude creates suggested subtasks
ai-trackdown create issue "Design API rate limiting strategy" --epic=EPIC-003 --labels=architecture,api
ai-trackdown create issue "Implement rate limiting middleware" --epic=EPIC-003 --labels=implementation,backend
ai-trackdown create issue "Add rate limiting configuration" --epic=EPIC-003 --labels=configuration,devops
```

### GPT-4 Code Generation Workflow

**Scenario:** GPT-4 assisting with code implementation

```bash
# Developer requests AI assistance for specific task
ai-trackdown show TASK-124 --format=json > task_context.json

# GPT-4 analyzes task requirements
ai-trackdown analyze TASK-124 --estimate-tokens --risk-analysis

# Record planning tokens
ai-trackdown tokens add TASK-124 --agent=gpt4 --count=892 --purpose="Code planning and architecture review"

# After implementation assistance
ai-trackdown tokens add TASK-124 --agent=gpt4 --count=2156 --purpose="Code generation and debugging"

# Update task with AI-generated content markers
ai-trackdown update TASK-124 --add-labels=ai-assisted --comment="Implementation completed with GPT-4 assistance"
```

### Multi-Agent Collaboration

**Scenario:** Multiple AI agents working on related tasks

```bash
# Claude analyzes requirements
ai-trackdown context ISSUE-067 --for=claude
ai-trackdown tokens add ISSUE-067 --agent=claude --count=1534 --purpose="Requirements analysis"

# GPT-4 handles implementation
ai-trackdown context ISSUE-067 --for=gpt4 --include-related
ai-trackdown tokens add ISSUE-067 --agent=gpt4 --count=2847 --purpose="Code implementation"

# Copilot assists with testing
ai-trackdown tokens add ISSUE-067 --agent=copilot --count=756 --purpose="Test generation"

# Check total token usage across agents
ai-trackdown show ISSUE-067 --include-tokens
```

### AI Context Optimization

**Scenario:** Optimizing AI context for token efficiency

```bash
# Generate minimal context for quick queries
ai-trackdown context TASK-089 --for=claude --max-tokens=1000

# Generate comprehensive context for complex analysis
ai-trackdown context EPIC-002 --for=claude --include-related --depth=3

# Check context size before sending to AI
ai-trackdown generate context ISSUE-045 --dry-run | wc -w

# Generate project-wide AI context
ai-trackdown generate llms-txt --include-closed=false
```

---

## Team Collaboration Scenarios

### Sprint Planning Meeting

**Scenario:** Team planning next sprint iteration

```bash
# Team lead reviews epic progress
ai-trackdown list --type=epic --status=in-progress

# Check story point allocation
ai-trackdown list --epic=EPIC-003 --format=json | jq '[.[] | {id, estimate}] | add'

# Review token budget usage across epics
ai-trackdown tokens report --by=epic --include-costs

# Identify tasks ready for assignment
ai-trackdown list --status=open --assignee=@unassigned
```

### Code Review Process

**Scenario:** Team conducting code reviews with AI assistance

```bash
# Reviewer gets context for the task being reviewed
ai-trackdown show ISSUE-042 --include-activity --include-related

# AI provides code review assistance
ai-trackdown analyze ISSUE-042 --risk-analysis
ai-trackdown tokens add ISSUE-042 --agent=claude --count=1823 --purpose="Code review analysis"

# Update task status after review
ai-trackdown update ISSUE-042 --status=done --comment="Code review passed, merged to main"

# Record review completion
ai-trackdown update ISSUE-042 --add-labels=reviewed --assignee=@original-dev
```

### Cross-team Dependencies

**Scenario:** Managing dependencies between teams

```bash
# Frontend team checks backend API readiness
ai-trackdown list --labels=api --status=done --epic=EPIC-002

# Backend team updates API completion status
ai-trackdown update ISSUE-089 --status=done --comment="API endpoints completed and documented"

# Frontend team gets unblocked
ai-trackdown update ISSUE-091 --status=open --comment="Unblocked by completion of ISSUE-089"

# Check cascade effects
ai-trackdown list --labels=depends-on-api --status=blocked
```

### Knowledge Sharing Session

**Scenario:** Team sharing knowledge and updating documentation

```bash
# Identify tasks with significant AI context
ai-trackdown list --has-ai-context --epic=EPIC-001

# Extract learnings from completed tasks
ai-trackdown list --status=done --updated-after="last week" --format=markdown

# Update task documentation based on learnings
ai-trackdown update ISSUE-023 --comment="Added architectural decisions and lessons learned to task description"

# Generate project knowledge export
ai-trackdown export markdown --include-tokens --include-activity
```

---

## Token Management Workflows

### Project Budget Planning

**Scenario:** Project manager setting up token budgets for new epic

```bash
# Set initial epic budget
ai-trackdown tokens budget set EPIC-004 100000

# Configure budget alerts
ai-trackdown tokens budget alert 0.8  # Alert at 80% usage

# Estimate token requirements for epic
ai-trackdown analyze EPIC-004 --estimate-tokens

# Check overall project token allocation
ai-trackdown tokens report --by=epic --include-costs --budget-warnings
```

### Weekly Token Review

**Scenario:** Weekly review of token usage and costs

```bash
# Generate weekly token report
ai-trackdown tokens report --period=week --by=agent --include-costs

# Check budget status for all epics
ai-trackdown tokens budget check --all

# Identify high token usage tasks
ai-trackdown tokens report --period=week --sort=usage --reverse --limit=10

# Export token data for finance
ai-trackdown tokens report --period=month --format=csv --include-costs > monthly_ai_costs.csv
```

### Token Optimization Analysis

**Scenario:** Analyzing and optimizing token usage patterns

```bash
# Identify tasks with unexpectedly high token usage
ai-trackdown list --format=json | jq '.[] | select(.token_usage.total > 5000) | {id, title, tokens: .token_usage.total}'

# Analyze token efficiency by agent
ai-trackdown tokens report --by=agent --period=month

# Check token usage trends
ai-trackdown tokens report --since="3 months ago" --by=week

# Identify optimization opportunities
ai-trackdown analyze --token-optimization --epic=EPIC-001
```

### Cost Tracking and Billing

**Scenario:** Tracking costs for client billing or department allocation

```bash
# Generate cost report by epic (for client billing)
ai-trackdown tokens report --by=epic --include-costs --period=month --format=json

# Track costs by team member
ai-trackdown tokens report --by=assignee --include-costs --period=quarter

# Export detailed cost breakdown
ai-trackdown export json --include-tokens --include-costs --period=month > billing_details.json

# Check against budget allocations
ai-trackdown tokens budget check --all --format=json | jq '.[] | {epic, budget, used, percentage, status}'
```

---

## Integration Sync Patterns

### GitHub Issues Synchronization

**Scenario:** Setting up and maintaining GitHub Issues sync

```bash
# Initial configuration
ai-trackdown config set sync.github.repo "myorg/myproject"
ai-trackdown config set sync.github.enabled true

# Test connection
ai-trackdown sync github --dry-run

# Perform initial full sync
ai-trackdown sync github --full

# Daily incremental sync
ai-trackdown sync github --since=1d

# Check sync status
ai-trackdown sync status github
```

### Jira Integration Workflow

**Scenario:** Bidirectional sync with Jira for enterprise workflow

```bash
# Configure Jira integration
ai-trackdown config set sync.jira.url "https://company.atlassian.net"
ai-trackdown config set sync.jira.project "PROJ"

# Import existing Jira issues
ai-trackdown sync jira --direction=pull --full

# Regular bidirectional sync
ai-trackdown sync jira --direction=both

# Handle sync conflicts
ai-trackdown sync jira --resolve-conflicts=prompt
```

### Multi-Platform Sync Strategy

**Scenario:** Managing sync across multiple platforms

```bash
# Check all platform sync status
ai-trackdown sync status

# Sync priority order: local -> GitHub -> Jira -> Linear
ai-trackdown sync github --direction=push
ai-trackdown sync jira --direction=push  
ai-trackdown sync linear --direction=push

# Verify sync consistency
ai-trackdown validate --sync-consistency

# Rollback if sync issues detected
ai-trackdown sync github --rollback-last
```

### Conflict Resolution Patterns

**Scenario:** Handling and resolving sync conflicts

```bash
# Detect conflicts
ai-trackdown sync github --dry-run

# Interactive conflict resolution
ai-trackdown sync github --resolve-conflicts=prompt

# Batch resolution strategies
ai-trackdown sync github --resolve-conflicts=local   # Prefer local
ai-trackdown sync github --resolve-conflicts=remote  # Prefer remote

# Manual conflict resolution
ai-trackdown show ISSUE-042 --conflicts
ai-trackdown update ISSUE-042 --resolve-conflict=use-local
```

---

## Advanced Usage Scenarios

### Automated CI/CD Integration

**Scenario:** Integration with CI/CD pipelines

```bash
# Pre-commit validation
ai-trackdown validate --strict --type=all

# Post-commit task updates
ai-trackdown parse-commit "$COMMIT_MESSAGE"

# Pre-deployment checks
ai-trackdown list --status=in-progress --epic=EPIC-003 --exit-if-found

# Post-deployment updates
ai-trackdown update EPIC-003 --add-labels=deployed --comment="Deployed to production"
```

### Bulk Operations and Migrations

**Scenario:** Bulk updates and data migrations

```bash
# Bulk status updates
ai-trackdown list --epic=EPIC-001 --status=open --format=json | \
  jq -r '.[].id' | \
  xargs -I {} ai-trackdown update {} --status=in-progress

# Mass label updates
ai-trackdown list --assignee=@olddev --format=json | \
  jq -r '.[].id' | \
  xargs -I {} ai-trackdown update {} --assignee=@newdev

# Migration from old system
csv_file="old_system_export.csv"
ai-trackdown import csv "$csv_file" --mapping="custom_mapping.yaml"

# Archive completed epics
ai-trackdown list --type=epic --status=done --created-before="6 months ago" | \
  ai-trackdown archive
```

### Custom Reporting and Analytics

**Scenario:** Custom analytics and reporting

```bash
# Velocity tracking
ai-trackdown list --status=done --updated-after="last month" --format=json | \
  jq '[.[] | .estimate] | add'

# Burndown data
ai-trackdown list --epic=EPIC-003 --format=json | \
  jq 'group_by(.status) | map({status: .[0].status, count: length})'

# Token efficiency analysis
ai-trackdown tokens report --period=month --format=json | \
  jq '.by_task | map(select(.tokens_per_story_point)) | sort_by(.tokens_per_story_point)'

# Custom dashboard data
ai-trackdown list --format=json > tasks.json
ai-trackdown tokens report --format=json > tokens.json
# Process with custom analytics script
python analytics_dashboard.py tasks.json tokens.json
```

### Multi-Project Management

**Scenario:** Managing multiple projects with shared resources

```bash
# Switch between projects
export AITASKTRACK_CONFIG="/path/to/project-a/.ai-trackdown/config.yaml"
ai-trackdown list --type=epic

export AITASKTRACK_CONFIG="/path/to/project-b/.ai-trackdown/config.yaml"
ai-trackdown list --type=epic

# Cross-project reporting
ai-trackdown tokens report --all-projects --by=assignee --period=week

# Shared resource allocation
ai-trackdown list --assignee=@shared-dev --all-projects --format=json | \
  jq 'group_by(.project) | map({project: .[0].project, tasks: length})'
```

### Emergency Response Patterns

**Scenario:** Handling production issues and urgent tasks

```bash
# Create urgent issue
ai-trackdown create issue "Production API down" \
  --priority=critical \
  --labels=production,urgent \
  --assignee=@oncall-dev

# Track incident response
ai-trackdown create epic "Incident Response - API Outage" \
  --owner=@incident-commander \
  --target-date="today"

# Real-time status updates
ai-trackdown update ISSUE-999 --status=investigating --comment="$(date): Checking logs"
ai-trackdown update ISSUE-999 --status=identified --comment="$(date): Found root cause in rate limiter"
ai-trackdown update ISSUE-999 --status=fixing --comment="$(date): Deploying hotfix"
ai-trackdown update ISSUE-999 --status=resolved --comment="$(date): Fix deployed, monitoring"

# Post-incident analysis
ai-trackdown create issue "Post-incident review - API outage" \
  --epic=EPIC-999 \
  --labels=postmortem,process-improvement
```

---

These usage patterns demonstrate real-world scenarios for ai-trackdown CLI usage, covering everything from daily development workflows to complex enterprise integration patterns. Each pattern includes practical command sequences and expected outcomes to guide implementation and testing.