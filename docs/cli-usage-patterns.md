# AI-Trackdown Manual Workflow Patterns

**Version:** 1.0  
**Status:** Workflow Specification  
**Last Updated:** January 2025

## Table of Contents

1. [Daily Developer Workflows](#daily-developer-workflows)
2. [AI Agent Interaction Patterns](#ai-agent-interaction-patterns)
3. [Team Collaboration Scenarios](#team-collaboration-scenarios)
4. [Token Management Workflows](#token-management-workflows)
5. [Integration Sync Patterns](#integration-sync-patterns)
6. [Advanced Manual Scenarios](#advanced-manual-scenarios)

---

## Daily Developer Workflows

### Morning Standup Preparation

**Scenario:** Developer preparing for daily standup meeting

```bash
# Check what I worked on yesterday
grep -l "assignee: @me" tasks/**/*.md | xargs grep -l "updated: $(date -d yesterday +%Y-%m-%d)"

# Show my current active tasks
grep -l "assignee: @me" tasks/**/*.md | xargs grep -l "status: in-progress"

# Quick status check for specific task
head -20 tasks/issues/ISSUE-042-fix-auth-bug.md | grep -E "(status|title)"

# Check if any tasks are blocked
grep -l "assignee: @me" tasks/**/*.md | xargs grep -l "status: blocked"
```

**Manual Review Process:**
```bash
# Create standup notes
echo "## Daily Standup - $(date +%Y-%m-%d)" > standup-notes.md
echo "" >> standup-notes.md
echo "### Yesterday:" >> standup-notes.md
grep -l "assignee: @me" tasks/**/*.md | while read file; do
    if grep -q "updated: $(date -d yesterday +%Y-%m-%d)" "$file"; then
        echo "- $(basename "$file" .md)" >> standup-notes.md
    fi
done
echo "" >> standup-notes.md
echo "### Today:" >> standup-notes.md
grep -l "status: in-progress" tasks/**/*.md | while read file; do
    if grep -q "assignee: @me" "$file"; then
        echo "- $(basename "$file" .md)" >> standup-notes.md
    fi
done
```

### Starting New Work

**Scenario:** Developer picking up a new task from the backlog

```bash
# Review available tasks
grep -l "status: todo" tasks/**/*.md | head -5

# Check task details manually
cat tasks/issues/ISSUE-045-implement-rate-limiting.md

# Update status to start work
vim tasks/issues/ISSUE-045-implement-rate-limiting.md
# Change: status: todo → status: in-progress
# Update: updated timestamp
# Add: progress notes
```

**Task Assignment Workflow:**
```yaml
# Update task frontmatter
---
id: ISSUE-045
status: in-progress  # Changed from todo
assignee: "@johndoe"
updated: "2025-01-07T09:30:00Z"  # Updated timestamp
---

# Add progress section to markdown body
## Progress Updates
- 2025-01-07 09:30: Started analysis and research
- Goal: Complete initial implementation by EOD
```

### End of Day Workflow

**Scenario:** Developer wrapping up work and updating task status

```bash
# Review today's work
grep -l "assignee: @me" tasks/**/*.md | xargs grep -l "updated: $(date +%Y-%m-%d)"

# Update task status and progress
vim tasks/issues/ISSUE-045-implement-rate-limiting.md

# Record any AI assistance used
# Update token usage in frontmatter

# Commit work with conventional commit message
git add .
git commit -m "feat(ISSUE-045): implement basic rate limiting middleware

- Add Redis-based rate limiter
- Configure per-route limits
- Add rate limit headers

Token-Usage: claude=892,gpt4=234"
```

**Status Update Template:**
```yaml
# In task frontmatter - update these fields:
status: in-progress  # or in-review if ready
updated: "2025-01-07T17:30:00Z"
token_usage:
  total: 1126
  by_agent:
    claude: 892
    gpt4: 234
  sessions:
    - date: "2025-01-07T14:00:00Z"
      agent: claude
      count: 892
      purpose: "Implementation guidance"
    - date: "2025-01-07T16:15:00Z"
      agent: gpt4  
      count: 234
      purpose: "Code review"
```

---

## AI Agent Interaction Patterns

### Context Preparation for AI Agents

**Scenario:** Preparing comprehensive context for AI assistance

```bash
# Generate context for specific epic
cat tasks/epics/EPIC-003-api-security.md > context.md
echo "" >> context.md
echo "## Related Issues" >> context.md
grep -l "epic: EPIC-003" tasks/issues/*.md | while read file; do
    echo "" >> context.md
    cat "$file" >> context.md
done

# Include relevant files mentioned in AI_CONTEXT blocks
grep -A 5 "AI_CONTEXT_START" tasks/**/*.md | grep -E "Files:|Dependencies:" >> context.md
```

**AI Context Maintenance:**
```markdown
# Update AI_CONTEXT blocks in task files
## AI Context
<!-- AI_CONTEXT_START -->
Rate limiting implementation for API security epic.
Key files: src/middleware/rateLimiter.js, config/redis.js
Dependencies: redis, express-rate-limit
Security requirements: Per-user limits, Redis clustering support
Current status: Basic implementation complete, need testing
<!-- AI_CONTEXT_END -->
```

### Token Usage Tracking

**Scenario:** AI agent self-reporting token usage

```bash
# AI agent script to update token usage
#!/bin/bash
TASK_FILE="$1"
AGENT_NAME="$2" 
TOKEN_COUNT="$3"
PURPOSE="$4"

# Read current frontmatter
python -c "
import yaml, sys
with open('$TASK_FILE') as f:
    content = f.read()
    parts = content.split('---')
    frontmatter = yaml.safe_load(parts[1])
    
    # Update token usage
    if 'token_usage' not in frontmatter:
        frontmatter['token_usage'] = {'total': 0, 'by_agent': {}, 'sessions': []}
    
    frontmatter['token_usage']['total'] += $TOKEN_COUNT
    frontmatter['token_usage']['by_agent']['$AGENT_NAME'] = \
        frontmatter['token_usage']['by_agent'].get('$AGENT_NAME', 0) + $TOKEN_COUNT
    
    frontmatter['token_usage']['sessions'].append({
        'date': '$(date -u +%Y-%m-%dT%H:%M:%SZ)',
        'agent': '$AGENT_NAME',
        'count': $TOKEN_COUNT,
        'purpose': '$PURPOSE'
    })
    
    # Write back to file
    with open('$TASK_FILE', 'w') as f:
        f.write('---\n')
        f.write(yaml.dump(frontmatter))
        f.write('---\n')
        f.write('---'.join(parts[2:]))
"
```

### AI-Driven Task Breakdown

**Scenario:** Using AI to help break down complex issues

```bash
# Prepare context for AI task breakdown
cat tasks/issues/ISSUE-067-complex-feature.md > breakdown-context.txt
echo "" >> breakdown-context.txt
echo "## Related Technical Context" >> breakdown-context.txt
grep -A 10 "Dependencies" tasks/issues/ISSUE-067-complex-feature.md >> breakdown-context.txt

# After AI analysis, create sub-tasks manually
cp .ai-trackdown/templates/task.md tasks/tasks/TASK-089-implement-auth-layer.md
cp .ai-trackdown/templates/task.md tasks/tasks/TASK-090-setup-data-models.md
cp .ai-trackdown/templates/task.md tasks/tasks/TASK-091-create-api-endpoints.md

# Link tasks to parent issue
vim tasks/tasks/TASK-089-implement-auth-layer.md
# Set: issue: ISSUE-067
```

---

## Team Collaboration Scenarios

### Sprint Planning Session

**Scenario:** Team planning new sprint with task estimation

```bash
# Gather all open issues for sprint planning
grep -l "status: todo" tasks/issues/*.md > sprint-candidates.txt

# Create sprint planning document
echo "# Sprint Planning - $(date +%Y-%m-%d)" > sprint-plan.md
echo "" >> sprint-plan.md
echo "## Backlog Items" >> sprint-plan.md

# Add issue summaries
while read file; do
    echo "" >> sprint-plan.md
    echo "### $(basename "$file" .md)" >> sprint-plan.md
    grep -E "title:|estimate:|assignee:" "$file" >> sprint-plan.md
done < sprint-candidates.txt
```

**Estimation Workflow:**
```bash
# Team reviews and updates estimates
for file in $(cat sprint-candidates.txt); do
    echo "Reviewing: $file"
    vim "$file"  # Update estimate field
    echo "Updated estimate for $(basename "$file")"
done

# Calculate sprint capacity
python -c "
import yaml, glob
total_points = 0
for file in open('sprint-candidates.txt').read().strip().split('\n'):
    if file:
        with open(file) as f:
            content = f.read()
            if '---' in content:
                data = yaml.safe_load(content.split('---')[1])
                total_points += data.get('estimate', 0)
print(f'Total story points: {total_points}')
"
```

### Code Review Process

**Scenario:** Reviewing code and updating task status

```bash
# Check tasks ready for review
grep -l "status: in-review" tasks/**/*.md

# Review specific task
git log --oneline --grep="ISSUE-045"  # Find related commits
git show <commit-hash>  # Review the implementation

# Update task after review
vim tasks/issues/ISSUE-045-implement-rate-limiting.md
# Add review comments in Progress Updates section
# Update status if approved: in-review → done
```

**Review Documentation Pattern:**
```markdown
## Progress Updates
- 2025-01-07 09:30: Started implementation
- 2025-01-07 17:30: Implementation complete, ready for review
- 2025-01-08 10:15: Code review by @teamlead
  - ✅ Implementation looks solid
  - ✅ Tests are comprehensive
  - ⚠️ Minor: Add error handling for Redis connection failures
- 2025-01-08 14:00: Addressed review comments
- 2025-01-08 14:30: Approved and merged
```

### Epic Progress Tracking

**Scenario:** Team tracking epic progress across multiple issues

```bash
# Epic progress calculation
epic_id="EPIC-003"
echo "Progress for $epic_id:"

# Count total and completed issues
total_issues=$(grep -l "epic: $epic_id" tasks/issues/*.md | wc -l)
done_issues=$(grep -l "epic: $epic_id" tasks/issues/*.md | xargs grep -l "status: done" | wc -l)

echo "Issues: $done_issues / $total_issues completed"
echo "Progress: $(( done_issues * 100 / total_issues ))%"

# Token usage for epic
echo "Token usage:"
grep -l "epic: $epic_id" tasks/issues/*.md | while read file; do
    python -c "
import yaml
with open('$file') as f:
    content = f.read()
    if '---' in content:
        data = yaml.safe_load(content.split('---')[1])
        usage = data.get('token_usage', {}).get('total', 0)
        if usage > 0:
            print(f'  {data[\"id\"]}: {usage} tokens')
"
done
```

---

## Token Management Workflows

### Budget Monitoring

**Scenario:** Monitoring token budget usage across projects

```bash
# Daily token usage report
echo "# Token Usage Report - $(date +%Y-%m-%d)" > token-report.md
echo "" >> token-report.md

# Calculate usage by epic
for epic_file in tasks/epics/*.md; do
    epic_id=$(grep "^id:" "$epic_file" | cut -d: -f2 | tr -d ' ')
    epic_title=$(grep "^title:" "$epic_file" | cut -d: -f2 | tr -d ' ')
    
    echo "## $epic_id: $epic_title" >> token-report.md
    
    # Sum tokens from all issues in epic
    total_tokens=0
    grep -l "epic: $epic_id" tasks/issues/*.md | while read issue_file; do
        tokens=$(python -c "
import yaml
with open('$issue_file') as f:
    content = f.read()
    if '---' in content:
        data = yaml.safe_load(content.split('---')[1])
        print(data.get('token_usage', {}).get('total', 0))
" 2>/dev/null || echo 0)
        total_tokens=$((total_tokens + tokens))
    done
    
    echo "- Total tokens: $total_tokens" >> token-report.md
done
```

### Cost Analysis

**Scenario:** Analyzing AI costs by agent and purpose

```bash
# Cost analysis script
python -c "
import yaml, glob
from collections import defaultdict

costs = {
    'claude': 0.000003,  # per token
    'gpt4': 0.00003,
    'gpt3': 0.000002
}

usage_by_agent = defaultdict(int)
usage_by_purpose = defaultdict(int)
total_cost = 0

for file in glob.glob('tasks/**/*.md', recursive=True):
    with open(file) as f:
        content = f.read()
        if '---' in content:
            try:
                data = yaml.safe_load(content.split('---')[1])
                token_usage = data.get('token_usage', {})
                
                # By agent
                for agent, count in token_usage.get('by_agent', {}).items():
                    usage_by_agent[agent] += count
                    if agent in costs:
                        total_cost += count * costs[agent]
                
                # By purpose
                for session in token_usage.get('sessions', []):
                    purpose = session.get('purpose', 'unknown')
                    count = session.get('count', 0)
                    usage_by_purpose[purpose] += count
            except:
                pass

print('=== Token Usage Analysis ===')
print(f'Total estimated cost: \${total_cost:.2f}')
print()
print('Usage by Agent:')
for agent, count in sorted(usage_by_agent.items()):
    cost = count * costs.get(agent, 0)
    print(f'  {agent}: {count:,} tokens (\${cost:.2f})')
print()
print('Usage by Purpose:')
for purpose, count in sorted(usage_by_purpose.items(), key=lambda x: x[1], reverse=True):
    print(f'  {purpose}: {count:,} tokens')
"
```

### Budget Alerts

**Scenario:** Setting up budget monitoring and alerts

```bash
# Budget monitoring script
#!/bin/bash
ALERT_THRESHOLD=0.8

for epic_file in tasks/epics/*.md; do
    # Extract budget and current usage
    budget=$(grep "token_budget:" "$epic_file" | cut -d: -f2 | tr -d ' ')
    epic_id=$(grep "^id:" "$epic_file" | cut -d: -f2 | tr -d ' ')
    
    if [ -n "$budget" ] && [ "$budget" -gt 0 ]; then
        # Calculate current usage
        current_usage=0
        grep -l "epic: $epic_id" tasks/issues/*.md | while read issue_file; do
            usage=$(python -c "
import yaml
with open('$issue_file') as f:
    content = f.read()
    if '---' in content:
        data = yaml.safe_load(content.split('---')[1])
        print(data.get('token_usage', {}).get('total', 0))
" 2>/dev/null || echo 0)
            current_usage=$((current_usage + usage))
        done
        
        # Check if over threshold
        threshold_tokens=$((budget * 80 / 100))  # 80% threshold
        if [ "$current_usage" -gt "$threshold_tokens" ]; then
            echo "⚠️  ALERT: $epic_id approaching budget limit"
            echo "   Usage: $current_usage / $budget tokens ($(( current_usage * 100 / budget ))%)"
        fi
    fi
done
```

---

## Integration Sync Patterns

### Manual GitHub Sync

**Scenario:** Synchronizing AI-Trackdown issues with GitHub Issues

```bash
# Export issues to GitHub-compatible format
python -c "
import yaml, glob, json

github_issues = []
for file in glob.glob('tasks/issues/*.md'):
    with open(file) as f:
        content = f.read()
        if '---' in content:
            data = yaml.safe_load(content.split('---')[1])
            body = '---'.join(content.split('---')[2:]).strip()
            
            # Format for GitHub
            issue = {
                'title': data.get('title'),
                'body': body,
                'labels': data.get('labels', []),
                'assignees': [data.get('assignee', '').replace('@', '')] if data.get('assignee') else [],
                'ai_trackdown_id': data.get('id')
            }
            github_issues.append(issue)

print(json.dumps(github_issues, indent=2))
" > github-export.json

# Use GitHub CLI to create issues
cat github-export.json | jq -r '.[] | @json' | while read issue; do
    echo "$issue" | jq -r '"Creating: " + .title'
    # gh issue create --title "$(echo "$issue" | jq -r .title)" --body "$(echo "$issue" | jq -r .body)"
done
```

### Jira Integration Pattern

**Scenario:** Syncing with Jira using REST API

```bash
# Prepare Jira export
python -c "
import yaml, glob, json

jira_issues = []
for file in glob.glob('tasks/issues/*.md'):
    with open(file) as f:
        content = f.read()
        if '---' in content:
            data = yaml.safe_load(content.split('---')[1])
            body = '---'.join(content.split('---')[2:]).strip()
            
            # Format for Jira
            issue = {
                'fields': {
                    'project': {'key': 'PROJ'},
                    'summary': data.get('title'),
                    'description': body,
                    'issuetype': {'name': 'Story'},
                    'priority': {'name': data.get('priority', 'Medium').title()},
                    'labels': data.get('labels', [])
                },
                'ai_trackdown_id': data.get('id')
            }
            jira_issues.append(issue)

print(json.dumps(jira_issues, indent=2))
" > jira-export.json

# Manual sync with Jira (using curl or Jira CLI)
# curl -X POST -H "Content-Type: application/json" -d @issue.json "$JIRA_URL/rest/api/2/issue/"
```

### Bidirectional Sync Tracking

**Scenario:** Maintaining sync state between systems

```yaml
# Add sync tracking to issue frontmatter
sync:
  github: 1234      # GitHub issue number
  jira: "PROJ-567"  # Jira ticket key
  last_sync: "2025-01-07T14:30:00Z"
  sync_status: "synced"  # synced, pending, conflict
```

---

## Advanced Manual Scenarios

### Bulk Operations

**Scenario:** Mass updating tasks for sprint transitions

```bash
# Move all completed tasks to archive
mkdir -p archive/tasks/issues/$(date +%Y-%m)
grep -l "status: done" tasks/issues/*.md | while read file; do
    mv "$file" "archive/tasks/issues/$(date +%Y-%m)/"
done

# Bulk assignee updates
find tasks/issues/ -name "*.md" -exec grep -l "assignee: @old-dev" {} \; | \
while read file; do
    sed -i 's/assignee: "@old-dev"/assignee: "@new-dev"/' "$file"
    sed -i "s/updated: .*/updated: \"$(date -u +%Y-%m-%dT%H:%M:%SZ)\"/" "$file"
done

# Sprint rollover - update epic target dates
find tasks/epics/ -name "*.md" | while read file; do
    if grep -q "target_date: 2025-01-15" "$file"; then
        sed -i 's/target_date: "2025-01-15"/target_date: "2025-01-29"/' "$file"
    fi
done
```

### Template Customization

**Scenario:** Creating project-specific templates

```bash
# Create security review template
cp .ai-trackdown/templates/issue.md .ai-trackdown/templates/security-review.md

# Customize template
cat >> .ai-trackdown/templates/security-review.md << 'EOF'

## Security Checklist
- [ ] Input validation review
- [ ] Authentication/authorization check
- [ ] Data encryption verification
- [ ] Audit logging implementation
- [ ] Penetration testing completed

## Security Tools Used
- [ ] SAST scan completed
- [ ] DAST scan completed
- [ ] Dependency vulnerability scan
- [ ] Manual code review

## Compliance Requirements
- [ ] OWASP Top 10 addressed
- [ ] Company security standards met
- [ ] Privacy requirements validated
EOF

# Use custom template
cp .ai-trackdown/templates/security-review.md tasks/issues/ISSUE-078-auth-security-review.md
vim tasks/issues/ISSUE-078-auth-security-review.md
```

### Complex Reporting

**Scenario:** Generating comprehensive project reports

```bash
# Project health report
#!/bin/bash
echo "# Project Health Report - $(date +%Y-%m-%d)" > health-report.md
echo "" >> health-report.md

# Epic summaries
echo "## Epic Progress" >> health-report.md
for epic_file in tasks/epics/*.md; do
    epic_id=$(grep "^id:" "$epic_file" | cut -d: -f2 | tr -d ' ')
    epic_title=$(grep "^title:" "$epic_file" | cut -d: -f2- | sed 's/^[[:space:]]*//')
    
    total_issues=$(grep -l "epic: $epic_id" tasks/issues/*.md | wc -l)
    done_issues=$(grep -l "epic: $epic_id" tasks/issues/*.md | xargs grep -l "status: done" 2>/dev/null | wc -l)
    
    if [ "$total_issues" -gt 0 ]; then
        progress=$(( done_issues * 100 / total_issues ))
        echo "- **$epic_id**: $epic_title ($done_issues/$total_issues - $progress%)" >> health-report.md
    fi
done

# Velocity tracking
echo "" >> health-report.md
echo "## Team Velocity" >> health-report.md
echo "- Completed this week: $(find tasks/ -name "*.md" -exec grep -l "status: done" {} \; | xargs grep -l "updated: $(date +%Y-%m-%d)" | wc -l)" >> health-report.md

# Token usage summary  
echo "" >> health-report.md
echo "## Token Usage Summary" >> health-report.md
python -c "
import yaml, glob
total = 0
for file in glob.glob('tasks/**/*.md', recursive=True):
    with open(file) as f:
        content = f.read()
        if '---' in content:
            try:
                data = yaml.safe_load(content.split('---')[1])
                total += data.get('token_usage', {}).get('total', 0)
            except: pass
print(f'- Total tokens used: {total:,}')
"
```

This comprehensive guide provides manual workflow patterns for effectively using AI-Trackdown as a documentation framework without relying on CLI automation.