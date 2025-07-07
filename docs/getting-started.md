# Getting Started with AI-Trackdown Documentation Framework

Welcome to AI-Trackdown! This guide will help you get up and running with revolutionary markdown-based task management for AI development teams.

## 🎯 What You'll Learn

By the end of this guide, you'll understand:
- Why AI-Trackdown is different from traditional task managers
- How to set up your first AI-native project using templates
- How to track AI token usage and costs manually
- How to leverage llms.txt for instant AI context
- How to integrate with existing development workflows

## 🚀 Quick Setup (5 minutes)

### Prerequisites

- Git (for version control)
- A text editor (VS Code, vim, etc.)
- Basic familiarity with markdown

### Step 1: Copy Templates

```bash
# Copy the AI-Trackdown templates to your project
cp -r /path/to/ai-trackdown/templates/* my-ai-project/

# Or download templates from repository
wget https://github.com/your-org/ai-trackdown/archive/templates.zip
unzip templates.zip -d my-ai-project/
```

### Step 2: Setup Your Project Structure

```bash
# Navigate to your project directory
cd my-ai-project

# Initialize git repository if needed
git init

# Create the recommended structure manually
mkdir -p .ai-trackdown tasks/epics tasks/issues tasks/tasks docs
```

The manual setup creates this structure:
```
my-ai-project/
├── .ai-trackdown/
│   ├── config.yaml          # Project configuration
│   └── llms.txt             # AI context index (manually maintained)
├── tasks/
│   ├── epics/               # High-level project goals
│   ├── issues/              # Development issues/features
│   └── tasks/               # Granular implementation tasks
├── docs/
│   └── llms-full.txt        # Complete project context
└── AI-TRACKDOWN.md             # Project overview dashboard
```

### Step 3: Create Your First Epic

An epic represents a major feature or project milestone. Copy the epic template:

```bash
# Copy epic template
cp .ai-trackdown/templates/epic.md tasks/epics/EPIC-001-user-authentication-system.md

# Edit the template with your project details
vim tasks/epics/EPIC-001-user-authentication-system.md
```

The epic template includes:
- Token budget tracking
- Success metrics
- Linked issues
- AI context markers

### Step 4: Add Issues to Your Epic

Issues are specific features or requirements within an epic. Copy and customize issue templates:

```bash
# Copy issue templates
cp .ai-trackdown/templates/issue.md tasks/issues/ISSUE-001-implement-oauth2-login.md
cp .ai-trackdown/templates/issue.md tasks/issues/ISSUE-002-add-password-reset-flow.md
cp .ai-trackdown/templates/issue.md tasks/issues/ISSUE-003-implement-two-factor-auth.md

# Update each issue file with specific requirements
```

### Step 5: Break Down Issues into Tasks

Tasks are granular implementation steps. Use the task template:

```bash
# Copy task templates
cp .ai-trackdown/templates/task.md tasks/tasks/TASK-001-create-login-form-component.md
cp .ai-trackdown/templates/task.md tasks/tasks/TASK-002-setup-oauth2-provider-configs.md
cp .ai-trackdown/templates/task.md tasks/tasks/TASK-003-implement-jwt-token-handling.md

# Customize each task with implementation details
```

### Step 6: Start Tracking AI Usage

This is where AI-Trackdown becomes revolutionary. Manually track token usage in task files:

```yaml
# In your task files, update the token_usage section:
token_usage:
  total: 1247
  by_agent:
    claude: 892
    gpt4: 355
  sessions:
    - date: 2025-01-07
      agent: claude
      count: 1247
      purpose: "Requirements analysis"
```

### Step 7: Maintain AI Context

Make your project instantly readable by AI agents by maintaining llms.txt files:

```bash
# Manually update the main llms.txt file
vim .ai-trackdown/llms.txt

# Include project overview, current priorities, and key files
# Follow the llms.txt standard format
```

## 🧠 Understanding the AI-First Approach

### Traditional vs AI-Trackdown Workflow

**Traditional Task Management:**
1. Human creates task in web UI
2. AI agent needs context via API
3. Context is incomplete or stale
4. Token usage is invisible
5. Knowledge is siloed

**AI-Trackdown Workflow:**
1. Task created in markdown (human or AI)
2. AI context embedded directly in files
3. llms.txt provides instant project understanding
4. Token usage tracked manually in task files
5. Knowledge lives with code in git

### The Power of Markdown-First

**Why Markdown?**
- **Human Readable**: No special tools needed to view/edit
- **AI Friendly**: Optimal token efficiency for LLMs
- **Git Native**: Version control, branching, merging just work
- **Tool Agnostic**: Works with any editor, IDE, or automation
- **Future Proof**: Will be readable decades from now

### Token Economics

AI-Trackdown introduces **token economics** to development through manual tracking:

```yaml
# Track costs per epic/issue/task in AI-TRACKDOWN.md
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

You can aggregate this data manually or create scripts to parse token usage from task files.

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

AI-Trackdown supports flexible status workflows through manual file updates:

```bash
# Update task status by editing the YAML frontmatter
vim tasks/issues/ISSUE-001-implement-oauth2-login.md

# Change status field manually:
# status: todo → in-progress → in-review → done

# Update assignee field:
# assignee: @developer

# Add/update labels:
# labels: [priority-high, security, authentication]

# Update estimates:
# estimate: 8
```

### Git Integration

AI-Trackdown integrates seamlessly with git workflows through conventional commits:

```bash
# Use conventional commit messages that reference tasks
git commit -m "feat(ISSUE-001): implement OAuth2 flow"

git commit -m "fix(ISSUE-001): resolve PKCE validation"

git commit -m "close(ISSUE-001): OAuth2 implementation complete"

# Manually update task status after commits
vim tasks/issues/ISSUE-001-implement-oauth2-login.md
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
/AI-TRACKDOWN.md: Project status dashboard

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
# AI agents can read llms.txt files directly
cat .ai-trackdown/llms.txt

# Or read specific task context
cat tasks/epics/EPIC-001-user-authentication-system.md
cat tasks/issues/ISSUE-001-implement-oauth2-login.md
```

**Token Tracking:**
```yaml
# AI agents can update token usage in task files manually
# Add to the task file's frontmatter:
token_usage:
  total: 234
  sessions:
    - date: 2025-01-07
      agent: claude
      count: 234
      operation: code-review
```

**Task Management:**
```bash
# AI agents can create new tasks using templates
cp .ai-trackdown/templates/task.md tasks/tasks/TASK-004-new-task.md

# Then customize the template with specific details
```

## 🔄 Integration with External Systems

### GitHub Issues Integration

Create configuration templates for GitHub integration:

```yaml
# .ai-trackdown/integrations/github.yaml
repository: myorg/myrepo
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

Use the mapping configuration to manually sync or create scripts for automated sync.

### Jira Integration

Create Jira mapping templates:

```yaml
# .ai-trackdown/integrations/jira.yaml
server: https://company.atlassian.net
project: AUTH
mapping:
  story_points: estimate
  epic_link: epic
  status_mapping:
    todo: "To Do"
    in-progress: "In Progress"
    done: "Done"
```

### Linear Integration

Create Linear configuration:

```yaml
# .ai-trackdown/integrations/linear.yaml
team: engineering
status_mapping:
  todo: "Backlog"
  in-progress: "In Progress"
  in-review: "In Review"
  done: "Done"
```

## 📊 Reporting and Analytics

### Token Usage Reports

Create manual reports by analyzing task files:

```bash
# Parse token usage from all task files
grep -r "token_usage:" tasks/ | sort

# Create weekly reports manually
# Or build scripts to aggregate data from YAML frontmatter
```

### Project Health

Monitor project status through dashboard files:

```bash
# Review project status in AI-TRACKDOWN.md
cat AI-TRACKDOWN.md

# Check epic progress manually
grep -A 10 "EPIC-001" tasks/epics/*.md

# Track velocity by reviewing completed tasks
grep "status: done" tasks/**/*.md
```

### AI Efficiency Metrics

Analyze AI usage patterns manually:

```bash
# Review token usage patterns across tasks
grep -r "agent:" tasks/ | sort | uniq -c

# Analyze context quality by reviewing AI_CONTEXT blocks
grep -A 5 "AI_CONTEXT_START" tasks/**/*.md
```

## 🛠️ Advanced Configuration

### Custom Templates

Create custom templates for your team:

```bash
# Copy base template and customize
cp .ai-trackdown/templates/issue.md .ai-trackdown/templates/security-review.md

# Edit the custom template for security reviews
vim .ai-trackdown/templates/security-review.md

# Use custom template by copying it
cp .ai-trackdown/templates/security-review.md tasks/issues/ISSUE-XXX-security-review.md
```

### Configuration Management

```yaml
# .ai-trackdown/config.yaml
project:
  name: "My AI Project"
  token_budget: 50000
  default_assignee: "@team-lead"
  
templates:
  epic: ".ai-trackdown/templates/epic.md"
  issue: ".ai-trackdown/templates/issue.md"
  task: ".ai-trackdown/templates/task.md"
  security_review: ".ai-trackdown/templates/security-review.md"
```

### Team Workflows

```yaml
# .ai-trackdown/config.yaml
workflows:
  development:
    statuses: [todo, in-progress, in-review, done]
    labels: [feature, bug, enhancement, security]
    assignees: ["@dev1", "@dev2", "@dev3"]
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

**"Invalid YAML frontmatter"**
```bash
# Validate YAML manually using yaml parsers
python -c "import yaml; yaml.safe_load(open('tasks/issues/ISSUE-001.md').read().split('---')[1])"

# Or use online YAML validators
```

**"Integration issues"**
```bash
# Check configuration files
cat .ai-trackdown/integrations/github.yaml

# Verify file permissions
ls -la .ai-trackdown/
```

**"Missing templates"**
```bash
# Ensure templates directory exists
ls .ai-trackdown/templates/

# Copy missing templates from the framework
cp -r /path/to/ai-trackdown/templates/* .ai-trackdown/templates/
```

**"Token tracking inconsistencies"**
```bash
# Manually verify token data in task files
grep -r "token_usage:" tasks/ | head -5

# Check for malformed YAML frontmatter
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