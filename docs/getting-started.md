# Getting Started with ai-trackdown

Welcome to ai-trackdown! This guide will help you get up and running with revolutionary markdown-based task management for AI development teams.

## 🎯 What You'll Learn

By the end of this guide, you'll understand:
- Why ai-trackdown is different from traditional task managers
- How to set up your first AI-native project
- How to track AI token usage and costs
- How to leverage llms.txt for instant AI context
- How to integrate with existing development workflows

## 🚀 Quick Start (5 minutes)

### Prerequisites

- Node.js 18.0.0 or higher
- Git (for version control)
- A text editor (VS Code, vim, etc.)
- Basic familiarity with markdown

### Step 1: Install ai-trackdown

```bash
# Global installation (recommended)
npm install -g ai-trackdown

# Verify installation
ai-trackdown --version
```

### Step 2: Initialize Your First Project

```bash
# Navigate to your project directory
cd my-ai-project

# Initialize ai-trackdown
ai-trackdown init

# Follow the interactive setup
```

The initialization creates this structure:
```
my-ai-project/
├── .ai-trackdown/
│   ├── config.yaml          # Project configuration
│   └── llms.txt             # AI context index (auto-generated)
├── tasks/
│   ├── epics/               # High-level project goals
│   ├── issues/              # Development issues/features
│   └── tasks/               # Granular implementation tasks
├── docs/
│   └── llms-full.txt        # Complete project context
└── TASKTRACK.md             # Project overview dashboard
```

### Step 3: Create Your First Epic

An epic represents a major feature or project milestone:

```bash
ai-trackdown create epic "User Authentication System"
```

This creates `tasks/epics/EPIC-001-user-authentication-system.md` with:
- Token budget tracking
- Success metrics
- Linked issues
- AI context markers

### Step 4: Add Issues to Your Epic

Issues are specific features or requirements within an epic:

```bash
ai-trackdown create issue "Implement OAuth2 login" --epic=EPIC-001
ai-trackdown create issue "Add password reset flow" --epic=EPIC-001
ai-trackdown create issue "Implement two-factor auth" --epic=EPIC-001
```

### Step 5: Break Down Issues into Tasks

Tasks are granular implementation steps:

```bash
ai-trackdown create task "Create login form component" --issue=ISSUE-001
ai-trackdown create task "Set up OAuth2 provider configs" --issue=ISSUE-001
ai-trackdown create task "Implement JWT token handling" --issue=ISSUE-001
```

### Step 6: Start Tracking AI Usage

This is where ai-trackdown becomes revolutionary:

```bash
# Record AI token usage for a task
ai-trackdown tokens add ISSUE-001 --agent=claude --count=1247 --purpose="Requirements analysis"

# Check token usage
ai-trackdown tokens report --period=week

# Set budget alerts
ai-trackdown tokens budget --epic=EPIC-001 --limit=50000 --alert-threshold=0.8
```

### Step 7: Generate AI Context

Make your project instantly readable by AI agents:

```bash
# Generate llms.txt files
ai-trackdown generate llms-txt

# View the generated context
cat .ai-trackdown/llms.txt
```

## 🧠 Understanding the AI-First Approach

### Traditional vs ai-trackdown Workflow

**Traditional Task Management:**
1. Human creates task in web UI
2. AI agent needs context via API
3. Context is incomplete or stale
4. Token usage is invisible
5. Knowledge is siloed

**ai-trackdown Workflow:**
1. Task created in markdown (human or AI)
2. AI context embedded directly in files
3. llms.txt provides instant project understanding
4. Token usage tracked automatically
5. Knowledge lives with code in git

### The Power of Markdown-First

**Why Markdown?**
- **Human Readable**: No special tools needed to view/edit
- **AI Friendly**: Optimal token efficiency for LLMs
- **Git Native**: Version control, branching, merging just work
- **Tool Agnostic**: Works with any editor, IDE, or automation
- **Future Proof**: Will be readable decades from now

### Token Economics

ai-trackdown introduces **token economics** to development:

```bash
# Track costs per epic/issue/task
ai-trackdown tokens report --by=epic

Token Usage Report (This Week)
============================
EPIC-001 (Auth System): 12,847 tokens ($0.39) - 25.7% of budget
├── ISSUE-001 (OAuth): 4,892 tokens ($0.15)
├── ISSUE-002 (Reset): 3,234 tokens ($0.10)
└── ISSUE-003 (2FA): 4,721 tokens ($0.14)

Top Token Consumers:
1. Claude (requirements): 8,456 tokens
2. GPT-4 (code review): 3,234 tokens
3. Copilot (implementation): 1,157 tokens
```

## 📝 Working with Tasks

### Task Anatomy

Every task file follows this structure:

```markdown
---
id: ISSUE-001
type: issue
title: Implement OAuth2 login
status: in-progress
epic: EPIC-001
assignee: @developer
created: 2025-01-07T10:00:00Z
updated: 2025-01-07T14:30:00Z
labels: [authentication, oauth, security]
estimate: 8
token_usage:
  total: 1247
  by_agent:
    claude: 892
    gpt4: 355
sync:
  github: 145
  jira: AUTH-234
---

# Implement OAuth2 login

## Description
Create secure OAuth2 authentication flow supporting Google and GitHub providers.

## Acceptance Criteria
- [ ] OAuth2 provider configuration
- [ ] Authorization code flow implementation
- [ ] Token exchange and validation
- [ ] User profile integration
- [x] Security review completed

## Technical Implementation
- Use PKCE flow for enhanced security
- Store tokens securely with httpOnly cookies
- Implement refresh token rotation

## AI Context
<!-- AI_CONTEXT_START -->
This implements OAuth2 authentication for the user management system.
Key files: src/auth/oauth.js, src/components/OAuthButton.tsx
Dependencies: passport-oauth2, jsonwebtoken
Security considerations: PKCE, CSRF protection, secure cookie handling
<!-- AI_CONTEXT_END -->

## Related Tasks
- Blocks: TASK-003, TASK-004
- Blocked by: TASK-001
- Related: ISSUE-003 (2FA integration)
```

### Status Management

ai-trackdown supports flexible status workflows:

```bash
# Update task status
ai-trackdown update ISSUE-001 --status=in-progress
ai-trackdown update ISSUE-001 --status=in-review
ai-trackdown update ISSUE-001 --status=done

# Assign tasks
ai-trackdown assign ISSUE-001 @developer

# Add labels
ai-trackdown label ISSUE-001 --add=priority-high,security

# Update estimates
ai-trackdown estimate ISSUE-001 --points=8
```

### Git Integration

ai-trackdown integrates seamlessly with git workflows:

```bash
# Git hooks automatically update tasks
git commit -m "feat(ISSUE-001): implement OAuth2 flow"
# → Updates ISSUE-001 status to 'in-review'

git commit -m "fix(ISSUE-001): resolve PKCE validation"
# → Adds commit reference to ISSUE-001

git commit -m "close(ISSUE-001): OAuth2 implementation complete"
# → Sets ISSUE-001 status to 'done'
```

### Branch Naming Convention

```bash
# Feature branches
git checkout -b feature/ISSUE-001-oauth2-login

# Bug fixes
git checkout -b bugfix/TASK-042-fix-token-validation

# Epic branches
git checkout -b epic/EPIC-001-authentication-system
```

## 🤖 AI Agent Integration

### Making Your Project AI-Discoverable

The llms.txt standard makes your project instantly understandable:

```txt
# .ai-trackdown/llms.txt
# ai-trackdown Project Index
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
/TASKTRACK.md: Project status dashboard

## Current Priorities
1. EPIC-001: User Authentication (in-progress, 70% complete)
   - ISSUE-001: OAuth2 login (in-progress)
   - ISSUE-002: Password reset (pending)
   - ISSUE-003: Two-factor auth (pending)

## Integration Status
GitHub: Synced (2025-01-07T14:55:00Z)
Token Budget: 50,000 (25.7% used)
```

### AI Agent Workflows

**Context Retrieval:**
```bash
# Get AI-optimized context for specific work
ai-trackdown context EPIC-001 --for=claude
ai-trackdown context ISSUE-001 --depth=3 --include-dependencies
```

**Token Tracking:**
```bash
# AI agents can self-report usage
ai-trackdown tokens add ISSUE-001 --agent=claude --count=234 --operation=code-review

# Automatic tracking via git hooks
# (when AI agents commit with proper attribution)
```

**Task Suggestions:**
```bash
# AI can suggest task breakdowns
ai-trackdown analyze ISSUE-001 --suggest-tasks --model=gpt4
```

## 🔄 Sync with External Systems

### GitHub Issues Integration

```bash
# Setup GitHub sync
ai-trackdown sync setup github --repo=myorg/myrepo

# Bidirectional sync
ai-trackdown sync github --full

# Incremental sync
ai-trackdown sync github --since=1h
```

**Mapping Configuration:**
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
```

### Jira Integration

```bash
# Setup Jira sync
ai-trackdown sync setup jira --url=https://company.atlassian.net --project=AUTH

# Sync with custom field mapping
ai-trackdown sync jira --map-story-points=estimate
```

### Linear Integration

```bash
# Setup Linear sync
ai-trackdown sync setup linear --team=engineering

# Sync with status mapping
ai-trackdown sync linear --status-map="{in-progress: 'In Progress', done: 'Done'}"
```

## 📊 Reporting and Analytics

### Token Usage Reports

```bash
# Weekly token usage
ai-trackdown tokens report --period=week --by=agent

# Cost analysis
ai-trackdown tokens cost --epic=EPIC-001 --breakdown

# Budget alerts
ai-trackdown tokens alert --threshold=0.8 --email=dev@company.com
```

### Project Health

```bash
# Project status overview
ai-trackdown status

# Epic progress
ai-trackdown progress --epic=EPIC-001

# Velocity tracking
ai-trackdown velocity --period=month --by-assignee
```

### AI Efficiency Metrics

```bash
# Token efficiency analysis
ai-trackdown analyze tokens --optimize-suggestions

# Context quality metrics
ai-trackdown analyze context --completeness-score
```

## 🛠️ Advanced Configuration

### Custom Templates

Create custom templates for your team:

```bash
# Create custom issue template
ai-trackdown template create issue security-review

# Use custom template
ai-trackdown create issue "Security review" --template=security-review
```

### Automation Rules

```yaml
# .ai-trackdown/config.yaml
automation:
  rules:
    - trigger: commit
      pattern: "fix\\((.+)\\):"
      action: update_status
      status: in-review
      
    - trigger: token_usage
      threshold: 0.9
      action: alert
      channels: [slack, email]
```

### Team Workflows

```yaml
# .ai-trackdown/config.yaml
workflows:
  development:
    statuses: [todo, in-progress, in-review, done]
    transitions:
      todo -> in-progress: auto
      in-progress -> in-review: requires_commit
      in-review -> done: requires_approval
```

## 🎓 Best Practices

### 1. Token Budget Management
- Set realistic token budgets per epic
- Monitor usage weekly
- Use token reports to optimize AI interactions

### 2. Context Quality
- Write clear, concise AI context blocks
- Update context as requirements evolve
- Include relevant file paths and dependencies

### 3. Git Integration
- Use conventional commit messages
- Reference task IDs in all commits
- Keep task branches focused and short-lived

### 4. Team Collaboration
- Establish consistent labeling conventions
- Use assignee mentions consistently
- Regular sync with external systems

### 5. AI Agent Guidelines
- Train agents to update token usage
- Provide clear task context requirements
- Establish agent permission boundaries

## 🚨 Troubleshooting

### Common Issues

**"ai-trackdown command not found"**
```bash
# Reinstall globally
npm uninstall -g ai-trackdown
npm install -g ai-trackdown
```

**"Invalid YAML frontmatter"**
```bash
# Validate task files
ai-trackdown validate --fix-formatting
```

**"Sync conflicts with GitHub"**
```bash
# Check sync status
ai-trackdown sync status

# Resolve conflicts manually
ai-trackdown sync resolve --interactive
```

**"Token tracking not working"**
```bash
# Check git hooks installation
ai-trackdown hooks status

# Reinstall hooks
ai-trackdown hooks install --force
```

### Getting Help

- **Documentation**: Browse `/docs` folder for detailed guides
- **Examples**: Check `/examples` for common configurations
- **GitHub Issues**: Report bugs and request features
- **Discussions**: Ask questions and share best practices

## 🎯 Next Steps

Now that you understand the basics:

1. **Set up your first real project** with ai-trackdown
2. **Configure integrations** with your existing tools
3. **Establish team workflows** and conventions
4. **Start tracking token usage** and optimize AI costs
5. **Explore advanced features** like automation and custom templates

### Learning Resources

- [Architecture Guide](architecture.md) - Deep dive into system design
- [Integration Setup](integrations.md) - Complete integration configurations
- [Token Tracking Guide](token-tracking.md) - Master AI cost optimization
- [Best Practices](best-practices.md) - Team workflow recommendations
- [API Reference](api-reference.md) - Complete CLI command reference

Welcome to the future of AI-native project management! 🚀