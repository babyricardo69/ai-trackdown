# ai-trackdown CLI Reference

Complete command-line interface reference for ai-trackdown.

## Installation and Setup

### Global Installation
```bash
npm install -g ai-trackdown
```

### Project Initialization
```bash
ai-trackdown init [options]
```

**Options:**
- `--force` - Overwrite existing configuration
- `--template <name>` - Use specific project template
- `--no-git-hooks` - Skip git hooks installation

**Examples:**
```bash
# Basic initialization
ai-trackdown init

# Force reinitialize existing project
ai-trackdown init --force

# Initialize with custom template
ai-trackdown init --template=enterprise
```

## Core Commands

### Status and Information

#### `ai-trackdown status [options]`
Display project status and overview.

**Options:**
- `--epic <id>` - Show status for specific epic
- `--team <name>` - Filter by team
- `--format <type>` - Output format (table, json, markdown)
- `--summary` - Show summary only

**Examples:**
```bash
# Project overview
ai-trackdown status

# Epic-specific status
ai-trackdown status --epic=EPIC-001

# Team status
ai-trackdown status --team=frontend

# JSON output for automation
ai-trackdown status --format=json
```

#### `ai-trackdown list [options]`
List and filter tasks.

**Options:**
- `--type <type>` - Filter by type (epic, issue, task)
- `--status <status>` - Filter by status
- `--assignee <user>` - Filter by assignee
- `--epic <id>` - Show items in specific epic
- `--labels <labels>` - Filter by labels (comma-separated)
- `--priority <level>` - Filter by priority
- `--created-since <date>` - Show items created since date
- `--limit <number>` - Limit number of results
- `--sort <field>` - Sort by field (created, updated, priority)
- `--format <type>` - Output format

**Examples:**
```bash
# List all open issues
ai-trackdown list --type=issue --status=open

# Your assigned tasks
ai-trackdown list --assignee=@me --status=in-progress

# High priority items
ai-trackdown list --priority=high

# Recent items
ai-trackdown list --created-since="2025-01-01" --sort=created

# JSON output for scripts
ai-trackdown list --format=json --status=open
```

### Creating Items

#### `ai-trackdown create epic <title> [options]`
Create a new epic.

**Options:**
- `--description <text>` - Epic description
- `--owner <user>` - Epic owner
- `--target-date <date>` - Target completion date
- `--token-budget <number>` - Token budget for epic
- `--labels <labels>` - Labels (comma-separated)
- `--template <name>` - Use specific template

**Examples:**
```bash
# Basic epic
ai-trackdown create epic "User Authentication System"

# Epic with full details
ai-trackdown create epic "Payment Integration" \
  --description="Integrate payment processing with Stripe" \
  --owner=@team-lead \
  --target-date="2025-03-01" \
  --token-budget=50000 \
  --labels="payment,security,high-priority"

# Epic from template
ai-trackdown create epic "Security Audit" --template=security-epic
```

#### `ai-trackdown create issue <title> [options]`
Create a new issue.

**Options:**
- `--epic <id>` - Parent epic
- `--description <text>` - Issue description
- `--assignee <user>` - Assignee
- `--estimate <points>` - Story point estimate
- `--priority <level>` - Priority (low, medium, high, critical)
- `--labels <labels>` - Labels (comma-separated)
- `--template <name>` - Use specific template

**Examples:**
```bash
# Basic issue
ai-trackdown create issue "Implement OAuth2 login"

# Issue with details
ai-trackdown create issue "Password reset flow" \
  --epic=EPIC-001 \
  --assignee=@developer \
  --estimate=5 \
  --priority=high \
  --labels="authentication,frontend"

# Issue from template
ai-trackdown create issue "Security review" --template=security-review
```

#### `ai-trackdown create task <title> [options]`
Create a new task.

**Options:**
- `--issue <id>` - Parent issue
- `--description <text>` - Task description
- `--assignee <user>` - Assignee
- `--estimate <hours>` - Time estimate in hours
- `--labels <labels>` - Labels (comma-separated)
- `--template <name>` - Use specific template

**Examples:**
```bash
# Basic task
ai-trackdown create task "Create login form component"

# Task with details
ai-trackdown create task "Implement JWT validation" \
  --issue=ISSUE-001 \
  --assignee=@developer \
  --estimate=4 \
  --labels="backend,security"
```

### Updating Items

#### `ai-trackdown update <id> [options]`
Update existing item properties.

**Options:**
- `--status <status>` - Update status
- `--assignee <user>` - Update assignee
- `--priority <level>` - Update priority
- `--estimate <points>` - Update estimate
- `--add-labels <labels>` - Add labels
- `--remove-labels <labels>` - Remove labels
- `--comment <text>` - Add comment
- `--target-date <date>` - Update target date (epics only)

**Examples:**
```bash
# Update status
ai-trackdown update ISSUE-001 --status=in-progress

# Assign to user
ai-trackdown update TASK-001 --assignee=@developer

# Add comment and update status
ai-trackdown update ISSUE-001 --status=in-review --comment="Ready for review"

# Update multiple properties
ai-trackdown update EPIC-001 \
  --status=in-progress \
  --target-date="2025-02-15" \
  --add-labels="high-priority"
```

#### `ai-trackdown assign <id> <user>`
Assign item to user.

**Examples:**
```bash
# Assign to specific user
ai-trackdown assign ISSUE-001 @developer

# Assign to yourself
ai-trackdown assign TASK-001 @me

# Unassign
ai-trackdown assign ISSUE-001 --unassign
```

#### `ai-trackdown close <id> [options]`
Close/complete an item.

**Options:**
- `--comment <text>` - Closing comment
- `--resolution <type>` - Resolution type (completed, duplicate, wontfix)

**Examples:**
```bash
# Simple close
ai-trackdown close TASK-001

# Close with comment
ai-trackdown close ISSUE-001 --comment="Implemented and tested"

# Close as duplicate
ai-trackdown close ISSUE-001 --resolution=duplicate --comment="Duplicate of ISSUE-005"
```

## Token Tracking

### Recording Token Usage

#### `ai-trackdown tokens add <id> [options]`
Record AI token usage for a task.

**Options:**
- `--agent <name>` - AI agent name (required)
- `--count <number>` - Token count (required)
- `--purpose <text>` - Purpose description
- `--cost-center <name>` - Cost center for billing
- `--input-tokens <number>` - Input tokens (if different from total)
- `--output-tokens <number>` - Output tokens (if different from total)

**Examples:**
```bash
# Basic token tracking
ai-trackdown tokens add ISSUE-001 --agent=claude --count=1247

# Detailed tracking
ai-trackdown tokens add ISSUE-001 \
  --agent=claude \
  --count=1247 \
  --purpose="Requirements analysis and API design" \
  --cost-center="backend-development"

# Separate input/output tracking
ai-trackdown tokens add ISSUE-001 \
  --agent=gpt4 \
  --input-tokens=500 \
  --output-tokens=800 \
  --purpose="Code generation"
```

#### `ai-trackdown tokens correct <id> [options]`
Correct previously recorded token usage.

**Options:**
- `--agent <name>` - AI agent name
- `--count <number>` - Correct token count
- `--reason <text>` - Correction reason

**Examples:**
```bash
# Correct token count
ai-trackdown tokens correct ISSUE-001 \
  --agent=claude \
  --count=1500 \
  --reason="API tracking error"
```

### Token Reporting

#### `ai-trackdown tokens report [options]`
Generate token usage reports.

**Options:**
- `--period <period>` - Time period (day, week, month, quarter, year)
- `--since <date>` - Start date
- `--until <date>` - End date
- `--by <grouping>` - Group by (agent, epic, assignee, cost-center)
- `--epic <id>` - Report for specific epic
- `--agent <name>` - Report for specific agent
- `--format <type>` - Output format (table, json, csv)
- `--detailed` - Include detailed breakdown

**Examples:**
```bash
# Weekly report
ai-trackdown tokens report --period=week

# Monthly report by epic
ai-trackdown tokens report --period=month --by=epic

# Custom date range
ai-trackdown tokens report \
  --since="2025-01-01" \
  --until="2025-01-31" \
  --by=agent

# Detailed CSV export
ai-trackdown tokens report --period=month --format=csv --detailed
```

#### `ai-trackdown tokens budget [options]`
Manage token budgets and alerts.

**Options:**
- `--epic <id>` - Set budget for epic
- `--limit <number>` - Budget limit
- `--alert-threshold <percent>` - Alert threshold (0.0-1.0)
- `--status` - Show budget status
- `--all-epics` - Show status for all epics

**Examples:**
```bash
# Set epic budget
ai-trackdown tokens budget --epic=EPIC-001 --limit=50000

# Set budget with alerts
ai-trackdown tokens budget \
  --epic=EPIC-001 \
  --limit=50000 \
  --alert-threshold=0.8

# Check budget status
ai-trackdown tokens budget --status --all-epics
```

## AI and Context Generation

### llms.txt Generation

#### `ai-trackdown generate llms-txt [options]`
Generate llms.txt files for AI context.

**Options:**
- `--full` - Generate complete context file
- `--minimal` - Generate minimal index only
- `--include-closed` - Include closed items
- `--depth <number>` - Context depth level
- `--output <file>` - Output file path

**Examples:**
```bash
# Standard generation
ai-trackdown generate llms-txt

# Full context with closed items
ai-trackdown generate llms-txt --full --include-closed

# Minimal index only
ai-trackdown generate llms-txt --minimal

# Custom output location
ai-trackdown generate llms-txt --output=docs/ai-context.txt
```

#### `ai-trackdown context <id> [options]`
Generate AI context for specific item.

**Options:**
- `--for <agent>` - Target AI agent
- `--depth <number>` - Include related items depth
- `--include-dependencies` - Include dependency context
- `--format <type>` - Output format (markdown, json, text)
- `--max-tokens <number>` - Maximum token limit

**Examples:**
```bash
# Basic context
ai-trackdown context EPIC-001

# Agent-specific context
ai-trackdown context ISSUE-001 --for=claude --depth=3

# Limited token context
ai-trackdown context TASK-001 --max-tokens=2000
```

### AI Analysis

#### `ai-trackdown analyze <target> [options]`
AI-powered analysis and suggestions.

**Options:**
- `--suggest-tasks` - Suggest task breakdown
- `--estimate` - Provide effort estimates
- `--dependencies` - Analyze dependencies
- `--risks` - Identify risks
- `--agent <name>` - AI agent to use
- `--budget <tokens>` - Token budget for analysis

**Examples:**
```bash
# Suggest task breakdown
ai-trackdown analyze ISSUE-001 --suggest-tasks

# Risk analysis
ai-trackdown analyze EPIC-001 --risks --agent=claude

# Dependency analysis
ai-trackdown analyze ISSUE-001 --dependencies --budget=1000
```

## Synchronization

### Setup Integrations

#### `ai-trackdown sync setup <platform> [options]`
Configure synchronization with external platforms.

**Platforms:** `github`, `jira`, `linear`

**GitHub Options:**
- `--repo <repo>` - Repository (org/repo)
- `--token <token>` - Personal access token
- `--auth <method>` - Authentication method (token, app)

**Jira Options:**
- `--url <url>` - Jira instance URL
- `--project <key>` - Project key
- `--username <user>` - Username
- `--token <token>` - API token

**Linear Options:**
- `--team <team>` - Team identifier
- `--token <token>` - API token

**Examples:**
```bash
# GitHub setup
ai-trackdown sync setup github \
  --repo=myorg/myrepo \
  --token="${GITHUB_TOKEN}"

# Jira setup
ai-trackdown sync setup jira \
  --url="https://company.atlassian.net" \
  --project="PROJ" \
  --username="user@company.com" \
  --token="${JIRA_TOKEN}"

# Linear setup
ai-trackdown sync setup linear \
  --team="engineering" \
  --token="${LINEAR_TOKEN}"
```

### Sync Operations

#### `ai-trackdown sync <platform> [options]`
Synchronize with external platform.

**Options:**
- `--full` - Full bidirectional sync
- `--push` - Push local changes only
- `--pull` - Pull remote changes only
- `--since <date>` - Sync changes since date
- `--items <ids>` - Sync specific items
- `--dry-run` - Show what would be synced

**Examples:**
```bash
# Full sync
ai-trackdown sync github --full

# Incremental sync
ai-trackdown sync github --since="2025-01-01"

# Push only
ai-trackdown sync github --push

# Dry run to preview changes
ai-trackdown sync github --dry-run
```

#### `ai-trackdown sync status [platform]`
Check synchronization status.

**Examples:**
```bash
# All platforms
ai-trackdown sync status

# Specific platform
ai-trackdown sync status github

# Detailed status
ai-trackdown sync status --detailed
```

### Conflict Resolution

#### `ai-trackdown sync conflicts [platform]`
List synchronization conflicts.

#### `ai-trackdown sync resolve [platform] [options]`
Resolve synchronization conflicts.

**Options:**
- `--interactive` - Interactive resolution
- `--auto` - Automatic resolution using strategy
- `--strategy <type>` - Resolution strategy (local, remote, merge)

**Examples:**
```bash
# List conflicts
ai-trackdown sync conflicts github

# Interactive resolution
ai-trackdown sync resolve github --interactive

# Auto-resolve with local preference
ai-trackdown sync resolve github --auto --strategy=local
```

## Configuration

### Configuration Management

#### `ai-trackdown config get <key>`
Get configuration value.

#### `ai-trackdown config set <key> <value>`
Set configuration value.

#### `ai-trackdown config list`
List all configuration.

**Examples:**
```bash
# Get project name
ai-trackdown config get project.name

# Set token budget
ai-trackdown config set project.default_epic_budget 50000

# List AI providers
ai-trackdown config get ai.providers
```

### Templates

#### `ai-trackdown template list`
List available templates.

#### `ai-trackdown template create <type> <name>`
Create custom template.

#### `ai-trackdown template edit <name>`
Edit existing template.

**Examples:**
```bash
# List templates
ai-trackdown template list

# Create custom issue template
ai-trackdown template create issue security-review

# Edit epic template
ai-trackdown template edit epic-default
```

## Git Integration

### Git Hooks

#### `ai-trackdown hooks install`
Install git hooks.

#### `ai-trackdown hooks uninstall`
Remove git hooks.

#### `ai-trackdown hooks status`
Check hook installation status.

**Examples:**
```bash
# Install hooks
ai-trackdown hooks install

# Check status
ai-trackdown hooks status

# Reinstall hooks
ai-trackdown hooks install --force
```

### Commit Integration

#### `ai-trackdown parse-commit <commit-hash>`
Parse commit message for task references.

**Examples:**
```bash
# Parse latest commit
ai-trackdown parse-commit HEAD

# Parse specific commit
ai-trackdown parse-commit abc123def
```

## Validation and Maintenance

### Validation

#### `ai-trackdown validate [options]`
Validate project data integrity.

**Options:**
- `--fix` - Automatically fix issues
- `--summary` - Show summary only
- `--type <type>` - Validate specific type only

**Examples:**
```bash
# Validate all data
ai-trackdown validate

# Validate and fix issues
ai-trackdown validate --fix

# Summary only
ai-trackdown validate --summary
```

### Import/Export

#### `ai-trackdown export [options]`
Export project data.

**Options:**
- `--format <type>` - Export format (json, csv, markdown)
- `--output <file>` - Output file
- `--include-closed` - Include closed items

**Examples:**
```bash
# Export to JSON
ai-trackdown export --format=json --output=backup.json

# Export to CSV
ai-trackdown export --format=csv --include-closed
```

#### `ai-trackdown import <file> [options]`
Import project data.

**Options:**
- `--format <type>` - Import format
- `--merge` - Merge with existing data
- `--dry-run` - Preview import

**Examples:**
```bash
# Import from JSON
ai-trackdown import backup.json --format=json

# Dry run import
ai-trackdown import data.csv --format=csv --dry-run
```

## Global Options

These options work with most commands:

- `--help, -h` - Show help
- `--version, -V` - Show version
- `--verbose, -v` - Verbose output
- `--quiet, -q` - Suppress non-error output
- `--config <file>` - Use specific config file
- `--no-color` - Disable colored output
- `--format <type>` - Output format (table, json, csv, markdown)

## Environment Variables

- `AI_TRACKDOWN_CONFIG` - Configuration file path
- `AI_TRACKDOWN_DEBUG` - Enable debug logging
- `GITHUB_TOKEN` - GitHub API token
- `JIRA_TOKEN` - Jira API token
- `LINEAR_TOKEN` - Linear API token
- `OPENAI_API_KEY` - OpenAI API key

## Exit Codes

- `0` - Success
- `1` - General error
- `2` - Configuration error
- `3` - Validation error
- `4` - Sync error
- `5` - Authentication error

## Examples and Workflows

### Daily Developer Workflow
```bash
# Check your work
ai-trackdown list --assignee=@me --status=open

# Start working on a task
ai-trackdown update TASK-001 --status=in-progress

# Record AI assistance
ai-trackdown tokens add TASK-001 --agent=claude --count=456 --purpose="implementation help"

# Complete the task
git commit -m "feat(TASK-001): implement feature

Token-Usage: claude=456"

# Task automatically updated via git hooks
ai-trackdown status TASK-001
```

### Sprint Planning
```bash
# Create new epic
ai-trackdown create epic "Q1 Feature Release" --token-budget=100000

# Break down into issues
ai-trackdown create issue "User dashboard" --epic=EPIC-001 --estimate=8
ai-trackdown create issue "Admin panel" --epic=EPIC-001 --estimate=13

# Generate AI context for planning
ai-trackdown generate llms-txt

# Check token budget allocation
ai-trackdown tokens budget --status --all-epics
```

### Weekly Reporting
```bash
# Token usage report
ai-trackdown tokens report --period=week --by=epic

# Progress report
ai-trackdown status --format=markdown > weekly-status.md

# Sync with external systems
ai-trackdown sync github --incremental
```

This comprehensive CLI reference provides all the commands and options needed to effectively use ai-trackdown in development workflows.