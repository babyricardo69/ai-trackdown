# ai-trackdown CLI Specification

**Version:** 1.0  
**Status:** Specification  
**Last Updated:** January 2025

## Table of Contents

1. [Overview](#overview)
2. [Installation & Setup](#installation--setup)
3. [Command Reference](#command-reference)
4. [Parameter Standards](#parameter-standards)
5. [Input/Output Formats](#inputoutput-formats)
6. [Error Handling](#error-handling)
7. [Configuration](#configuration)
8. [Environment Variables](#environment-variables)

---

## Overview

The ai-trackdown CLI (`ai-trackdown`) provides a comprehensive command-line interface for managing AI-native task tracking. It follows standard Unix conventions and provides both human-friendly and machine-parseable outputs.

### Design Principles

- **Consistency**: All commands follow consistent naming and parameter patterns
- **Composability**: Commands can be chained and piped for complex workflows
- **Discoverability**: Built-in help system with examples and guidance
- **AI-Friendly**: Structured outputs suitable for AI agent consumption
- **Git-Native**: Seamless integration with git workflows

### Command Structure

```
ai-trackdown <command> [subcommand] [options] [arguments]
```

Example:
```bash
ai-trackdown create issue "Fix login bug" --epic=EPIC-001 --assignee=@johndoe
```

---

## Installation & Setup

### Installation

```bash
# Via npm (recommended)
npm install -g ai-trackdown

# Via binary download
curl -L https://github.com/org/ai-trackdown/releases/latest/download/ai-trackdown-$(uname -s)-$(uname -m) -o /usr/local/bin/ai-trackdown
chmod +x /usr/local/bin/ai-trackdown

# Via Docker
docker run --rm -v $(pwd):/workspace ai-trackdown/cli:latest
```

### Verification

```bash
ai-trackdown --version
ai-trackdown help
```

---

## Command Reference

### Core Commands

#### `init` - Initialize Project

Initialize ai-trackdown in a new or existing project.

```bash
ai-trackdown init [options]
```

**Options:**
- `--name=<string>` - Project name (default: directory name)
- `--id-prefix=<string>` - Task ID prefix (default: derived from name)
- `--template=<string>` - Project template (basic|enterprise|minimal)
- `--force` - Overwrite existing configuration
- `--interactive` - Interactive setup wizard

**Examples:**
```bash
# Basic initialization
ai-trackdown init

# Custom project setup
ai-trackdown init --name="My Project" --id-prefix="PROJ"

# Interactive setup
ai-trackdown init --interactive

# Force reinitialize
ai-trackdown init --force
```

**Output:**
```
✓ Created .ai-trackdown/config.yaml
✓ Created tasks/ directory structure
✓ Generated initial llms.txt
✓ Installed git hooks
✓ ai-trackdown initialized successfully

Next steps:
  1. Create your first epic: ai-trackdown create epic "Project Setup"
  2. Configure integrations: ai-trackdown config sync
  3. View help: ai-trackdown help
```

**Exit Codes:**
- `0` - Success
- `1` - Already initialized (without --force)
- `2` - Invalid configuration
- `3` - Permission denied

#### `create` - Create Task Items

Create new epics, issues, or tasks.

```bash
ai-trackdown create <type> <title> [options]
```

**Types:**
- `epic` - High-level project epic
- `issue` - Development issue or story
- `task` - Granular implementation task

**Common Options:**
- `--id=<string>` - Custom ID (auto-generated if not provided)
- `--assignee=<string>` - Assignee (@username or email)
- `--labels=<list>` - Comma-separated labels
- `--description=<string>` - Initial description
- `--editor` - Open in default editor for detailed input
- `--template=<string>` - Use specific template

**Epic-Specific Options:**
- `--owner=<string>` - Epic owner
- `--target-date=<date>` - Target completion date
- `--token-budget=<number>` - Token budget allocation

**Issue/Task-Specific Options:**
- `--epic=<id>` - Parent epic ID
- `--issue=<id>` - Parent issue ID (for tasks)
- `--estimate=<number>` - Story point estimate
- `--status=<string>` - Initial status (default: open)
- `--priority=<string>` - Priority level (low|medium|high|critical)

**Examples:**
```bash
# Create epic
ai-trackdown create epic "User Authentication System" \
  --owner=@teamlead \
  --target-date=2025-02-15 \
  --token-budget=50000

# Create issue
ai-trackdown create issue "Implement login flow" \
  --epic=EPIC-001 \
  --assignee=@johndoe \
  --estimate=5 \
  --labels=authentication,frontend,priority-high

# Create task
ai-trackdown create task "Add OAuth2 integration" \
  --issue=ISSUE-001 \
  --estimate=3 \
  --assignee=@johndoe

# Interactive creation
ai-trackdown create issue --editor
```

**Output:**
```
✓ Created ISSUE-001: Implement login flow
  Epic: EPIC-001
  Assignee: @johndoe  
  Labels: authentication, frontend, priority-high
  File: tasks/issues/ISSUE-001-implement-login-flow.md

Next steps:
  1. Edit details: ai-trackdown edit ISSUE-001
  2. Add acceptance criteria: ai-trackdown update ISSUE-001 --acceptance-criteria
  3. View: ai-trackdown show ISSUE-001
```

#### `list` - List and Filter Items

List and filter tasks with various criteria.

```bash
ai-trackdown list [options]
```

**Filter Options:**
- `--type=<type>` - Filter by type (epic|issue|task)
- `--status=<status>` - Filter by status (open|in-progress|done|closed)
- `--assignee=<string>` - Filter by assignee (@me for current user)
- `--epic=<id>` - Filter by epic
- `--issue=<id>` - Filter by issue
- `--labels=<list>` - Filter by labels (comma-separated)
- `--has-ai-context` - Only items with AI context
- `--created-after=<date>` - Created after date
- `--updated-after=<date>` - Updated after date

**Display Options:**
- `--format=<format>` - Output format (table|json|markdown|csv)
- `--columns=<list>` - Custom column selection
- `--sort=<field>` - Sort by field (id|title|status|created|updated)
- `--reverse` - Reverse sort order
- `--limit=<number>` - Limit results

**Examples:**
```bash
# List all open issues
ai-trackdown list --type=issue --status=open

# List my tasks
ai-trackdown list --assignee=@me

# List items in specific epic
ai-trackdown list --epic=EPIC-001

# List with AI context
ai-trackdown list --has-ai-context --format=json

# Custom view
ai-trackdown list --columns=id,title,status,assignee --sort=updated --reverse
```

**Output Formats:**

**Table (default):**
```
ID        TYPE   TITLE                    STATUS      ASSIGNEE  LABELS
EPIC-001  epic   User Authentication      in-progress @teamlead auth,core
ISSUE-001 issue  Implement login flow     in-progress @johndoe  auth,frontend
TASK-001  task   Create login form        open        @johndoe  frontend
```

**JSON:**
```json
[
  {
    "id": "EPIC-001",
    "type": "epic",
    "title": "User Authentication System",
    "status": "in-progress",
    "assignee": "@teamlead",
    "labels": ["auth", "core"],
    "created": "2025-01-01T09:00:00Z",
    "updated": "2025-01-07T14:30:00Z"
  }
]
```

#### `show` - Display Item Details

Show detailed information about a specific item.

```bash
ai-trackdown show <id> [options]
```

**Options:**
- `--format=<format>` - Output format (markdown|json|yaml)
- `--include-activity` - Include activity log
- `--include-tokens` - Include token usage details
- `--include-related` - Include related items
- `--raw` - Show raw markdown file

**Examples:**
```bash
# Show issue details
ai-trackdown show ISSUE-001

# Show with full context
ai-trackdown show ISSUE-001 --include-activity --include-tokens --include-related

# Raw markdown
ai-trackdown show ISSUE-001 --raw
```

#### `update` - Update Item Properties

Update properties of existing items.

```bash
ai-trackdown update <id> [options]
```

**Options:**
- `--status=<status>` - Update status
- `--assignee=<string>` - Update assignee
- `--labels=<list>` - Update labels (comma-separated)
- `--add-labels=<list>` - Add labels without removing existing
- `--remove-labels=<list>` - Remove specific labels
- `--estimate=<number>` - Update estimate
- `--priority=<string>` - Update priority
- `--epic=<id>` - Change parent epic
- `--issue=<id>` - Change parent issue
- `--comment=<string>` - Add comment with update
- `--editor` - Open in editor for detailed updates

**Examples:**
```bash
# Update status
ai-trackdown update ISSUE-001 --status=in-progress

# Change assignee
ai-trackdown update ISSUE-001 --assignee=@newdev

# Add labels
ai-trackdown update ISSUE-001 --add-labels=priority-high,security

# Update with comment
ai-trackdown update ISSUE-001 --status=done --comment="Completed implementation"
```

#### `close` - Close Items

Close epics, issues, or tasks.

```bash
ai-trackdown close <id> [options]
```

**Options:**
- `--comment=<string>` - Closing comment
- `--reason=<string>` - Reason for closing (completed|wontfix|duplicate|invalid)
- `--duplicate-of=<id>` - If closing as duplicate
- `--force` - Force close even if has open dependencies

**Examples:**
```bash
# Close with comment
ai-trackdown close ISSUE-001 --comment="Feature implemented and tested"

# Close as duplicate
ai-trackdown close ISSUE-002 --reason=duplicate --duplicate-of=ISSUE-001

# Force close epic
ai-trackdown close EPIC-001 --force
```

### Token Management Commands

#### `tokens` - Token Tracking Operations

Manage and track AI token usage.

```bash
ai-trackdown tokens <subcommand> [options]
```

**Subcommands:**

##### `add` - Record Token Usage

```bash
ai-trackdown tokens add <id> --agent=<agent> --count=<number> [options]
```

**Options:**
- `--agent=<string>` - AI agent name (claude|gpt4|copilot)
- `--count=<number>` - Token count
- `--purpose=<string>` - Purpose description
- `--cost=<number>` - Cost override (auto-calculated if not provided)
- `--model=<string>` - Specific model used

**Examples:**
```bash
# Record token usage
ai-trackdown tokens add ISSUE-001 --agent=claude --count=892 --purpose="Requirements analysis"

# With cost override
ai-trackdown tokens add ISSUE-001 --agent=gpt4 --count=1500 --cost=0.45 --model=gpt-4-turbo
```

##### `report` - Generate Token Reports

```bash
ai-trackdown tokens report [options]
```

**Options:**
- `--period=<period>` - Time period (today|week|month|quarter|year)
- `--since=<date>` - Custom start date
- `--until=<date>` - Custom end date
- `--by=<grouping>` - Group by (agent|epic|issue|task)
- `--format=<format>` - Output format (table|json|csv)
- `--include-costs` - Include cost calculations
- `--budget-warnings` - Show budget warnings

**Examples:**
```bash
# Weekly report
ai-trackdown tokens report --period=week

# Epic breakdown
ai-trackdown tokens report --by=epic --include-costs

# Custom period
ai-trackdown tokens report --since=2025-01-01 --until=2025-01-31
```

##### `budget` - Budget Management

```bash
ai-trackdown tokens budget <subcommand> [options]
```

**Subcommands:**
- `set <id> <amount>` - Set budget for epic
- `check <id>` - Check budget status
- `alert <threshold>` - Configure budget alerts

**Examples:**
```bash
# Set epic budget
ai-trackdown tokens budget set EPIC-001 50000

# Check budget status
ai-trackdown tokens budget check EPIC-001
```

### Synchronization Commands

#### `sync` - External Platform Sync

Synchronize with external platforms (GitHub, Jira, Linear).

```bash
ai-trackdown sync <platform> [options]
```

**Platforms:**
- `github` - GitHub Issues
- `jira` - Jira Issues
- `linear` - Linear Issues
- `all` - All configured platforms

**Options:**
- `--full` - Full synchronization (default: incremental)
- `--dry-run` - Show what would be synced without making changes
- `--since=<duration>` - Sync items modified since (e.g., 1h, 1d)
- `--direction=<direction>` - Sync direction (push|pull|both)
- `--resolve-conflicts=<strategy>` - Conflict resolution (local|remote|prompt)

**Examples:**
```bash
# Sync with GitHub
ai-trackdown sync github

# Full sync all platforms
ai-trackdown sync all --full

# Dry run
ai-trackdown sync jira --dry-run

# Push only
ai-trackdown sync github --direction=push
```

#### `sync status` - Check Sync Status

```bash
ai-trackdown sync status [platform]
```

**Examples:**
```bash
# Check all sync status
ai-trackdown sync status

# Check specific platform
ai-trackdown sync status github
```

### AI Operations Commands

#### `generate` - Generate AI Content

Generate AI-optimized content and indexes.

```bash
ai-trackdown generate <type> [options]
```

**Types:**
- `llms-txt` - Generate llms.txt index files
- `context` - Generate AI context for specific items
- `summary` - Generate project summary

**Options:**
- `--force` - Force regeneration
- `--include-closed` - Include closed items
- `--depth=<number>` - Context depth level

**Examples:**
```bash
# Generate llms.txt
ai-trackdown generate llms-txt

# Generate context for epic
ai-trackdown generate context EPIC-001
```

#### `analyze` - AI Analysis Operations

Perform AI-powered analysis on tasks and projects.

```bash
ai-trackdown analyze <id> [options]
```

**Options:**
- `--suggest-tasks` - Suggest breakdown tasks
- `--estimate-tokens` - Estimate token requirements
- `--risk-analysis` - Analyze project risks
- `--dependencies` - Analyze dependencies

**Examples:**
```bash
# Suggest task breakdown
ai-trackdown analyze ISSUE-001 --suggest-tasks

# Token estimation
ai-trackdown analyze EPIC-001 --estimate-tokens
```

#### `context` - AI Context Operations

Generate and manage AI context for items.

```bash
ai-trackdown context <id> [options]
```

**Options:**
- `--for=<agent>` - Optimize for specific agent
- `--format=<format>` - Output format (markdown|json)
- `--include-related` - Include related items context
- `--max-tokens=<number>` - Maximum token limit

**Examples:**
```bash
# Generate context for Claude
ai-trackdown context ISSUE-001 --for=claude

# Full context with related items
ai-trackdown context EPIC-001 --include-related --max-tokens=5000
```

### Utility Commands

#### `config` - Configuration Management

Manage ai-trackdown configuration.

```bash
ai-trackdown config <subcommand> [options]
```

**Subcommands:**
- `get <key>` - Get configuration value
- `set <key> <value>` - Set configuration value
- `list` - List all configuration
- `edit` - Edit configuration file
- `validate` - Validate configuration

**Examples:**
```bash
# Get configuration
ai-trackdown config get sync.github.repo

# Set configuration
ai-trackdown config set project.default_epic_budget 100000

# Edit interactively
ai-trackdown config edit
```

#### `template` - Template Management

Manage task templates.

```bash
ai-trackdown template <subcommand> [options]
```

**Subcommands:**
- `list` - List available templates
- `show <name>` - Show template content
- `create <name>` - Create new template
- `edit <name>` - Edit existing template

#### `validate` - Validation Operations

Validate task files and project structure.

```bash
ai-trackdown validate [options]
```

**Options:**
- `--fix` - Attempt to fix validation errors
- `--type=<type>` - Validate specific type only
- `--strict` - Strict validation mode

#### `import` - Import Operations

Import tasks from external sources.

```bash
ai-trackdown import <source> [options]
```

**Sources:**
- `github` - Import from GitHub Issues
- `jira` - Import from Jira
- `csv` - Import from CSV file
- `json` - Import from JSON file

#### `export` - Export Operations

Export tasks to external formats.

```bash
ai-trackdown export <format> [options]
```

**Formats:**
- `json` - JSON export
- `csv` - CSV export
- `markdown` - Markdown export
- `pdf` - PDF report

---

## Parameter Standards

### Common Parameter Patterns

#### ID Parameters
- Format: `TYPE-NUMBER` (e.g., `EPIC-001`, `ISSUE-042`)
- Case-insensitive
- Auto-completion available

#### User References
- Format: `@username` or `email@domain.com`
- Special values: `@me`, `@unassigned`

#### Date Parameters
- ISO 8601 format: `YYYY-MM-DDTHH:MM:SSZ`
- Relative: `1d`, `1w`, `1m` (1 day, 1 week, 1 month)
- Human-readable: `today`, `tomorrow`, `next week`

#### Label Parameters
- Comma-separated lists: `label1,label2,label3`
- No spaces around commas
- Case-sensitive

#### Status Values
- Standard statuses: `open`, `in-progress`, `in-review`, `done`, `closed`
- Custom statuses configured per project

#### Priority Values
- Standard priorities: `low`, `medium`, `high`, `critical`
- Numeric equivalents: `1`, `2`, `3`, `4`

### Global Options

Available for all commands:

- `--help, -h` - Show help for command
- `--version, -v` - Show version information
- `--config=<path>` - Use specific config file
- `--verbose, -V` - Verbose output
- `--quiet, -q` - Suppress non-essential output
- `--no-color` - Disable colored output
- `--json` - JSON output format
- `--dry-run` - Show what would happen without executing

---

## Input/Output Formats

### Input Formats

#### Interactive Editor Format
When using `--editor` option, opens template in default editor:

```markdown
# Title: [Required]
Brief descriptive title

# Description: [Optional]
Detailed description of the task

# Acceptance Criteria: [Optional]
- [ ] Criterion 1
- [ ] Criterion 2

# Technical Notes: [Optional]
Implementation details and considerations

# Labels: [Optional]
label1, label2, label3

# Additional Properties: [Optional]
assignee: @username
estimate: 5
priority: high
```

#### Bulk Import Format (CSV)
```csv
type,title,description,epic,assignee,labels,estimate,priority
issue,"Login flow","Implement user authentication",EPIC-001,@johndoe,"auth,frontend",5,high
task,"Create form","Create login form component",ISSUE-001,@johndoe,"frontend",3,medium
```

#### JSON Import Format
```json
{
  "tasks": [
    {
      "type": "issue",
      "title": "Login flow",
      "description": "Implement user authentication",
      "epic": "EPIC-001",
      "assignee": "@johndoe",
      "labels": ["auth", "frontend"],
      "estimate": 5,
      "priority": "high"
    }
  ]
}
```

### Output Formats

#### Table Format (Default)
```
ID        TYPE   TITLE                    STATUS      ASSIGNEE  LABELS
EPIC-001  epic   User Authentication      in-progress @teamlead auth,core
ISSUE-001 issue  Implement login flow     in-progress @johndoe  auth,frontend
```

#### JSON Format
```json
{
  "tasks": [
    {
      "id": "EPIC-001",
      "type": "epic",
      "title": "User Authentication System",
      "status": "in-progress",
      "assignee": "@teamlead",
      "labels": ["auth", "core"],
      "metadata": {
        "created": "2025-01-01T09:00:00Z",
        "updated": "2025-01-07T14:30:00Z",
        "token_usage": {
          "total": 12847,
          "by_agent": {
            "claude": 8234,
            "gpt4": 4613
          }
        }
      }
    }
  ]
}
```

#### Markdown Format
```markdown
# EPIC-001: User Authentication System

**Status:** in-progress  
**Assignee:** @teamlead  
**Labels:** auth, core  
**Created:** 2025-01-01T09:00:00Z  
**Updated:** 2025-01-07T14:30:00Z  

## Description
Complete authentication and authorization system...

## Token Usage
- Total: 12,847 tokens
- Claude: 8,234 tokens  
- GPT-4: 4,613 tokens
```

#### CSV Format
```csv
id,type,title,status,assignee,labels,created,updated
EPIC-001,epic,User Authentication System,in-progress,@teamlead,"auth,core",2025-01-01T09:00:00Z,2025-01-07T14:30:00Z
```

---

## Error Handling

### Error Categories

#### User Input Errors (Exit Code 1)
- Invalid command syntax
- Missing required parameters
- Invalid parameter values
- Malformed input data

#### System Errors (Exit Code 2)
- File system permission issues
- Network connectivity problems
- External service unavailable
- Configuration file errors

#### Validation Errors (Exit Code 3)
- Task validation failures
- Constraint violations
- Data integrity issues
- Sync conflicts

#### Not Found Errors (Exit Code 4)
- Task ID not found
- File not found
- Configuration not found
- Template not found

### Error Output Format

```json
{
  "error": {
    "code": "TASK_NOT_FOUND",
    "message": "Task ISSUE-999 not found",
    "details": {
      "id": "ISSUE-999",
      "searched_paths": [
        "tasks/issues/ISSUE-999-*.md"
      ]
    },
    "suggestions": [
      "Check the task ID: ai-trackdown list --type=issue",
      "Create new task: ai-trackdown create issue \"Task title\""
    ]
  }
}
```

### Error Messages

#### User-Friendly Messages
```
Error: Task ISSUE-999 not found

The task ID 'ISSUE-999' does not exist in this project.

Suggestions:
  • List all issues: ai-trackdown list --type=issue
  • Create new issue: ai-trackdown create issue "Your title"
  • Check task ID format: ISSUE-XXX or TASK-XXX
```

#### Machine-Parseable Messages
```
ERROR:TASK_NOT_FOUND:ISSUE-999:Task ISSUE-999 not found
```

### Common Error Scenarios

#### Invalid Task ID
```bash
$ ai-trackdown show INVALID-ID
Error: Invalid task ID format 'INVALID-ID'
Expected format: TYPE-NUMBER (e.g., ISSUE-001, TASK-042)
```

#### Missing Configuration
```bash
$ ai-trackdown sync github
Error: GitHub integration not configured
Run: ai-trackdown config set sync.github.repo "owner/repo"
```

#### Sync Conflicts
```bash
$ ai-trackdown sync github
Warning: Sync conflicts detected
- ISSUE-001: Local status 'in-progress', remote status 'done'
- ISSUE-002: Local assignee '@johndoe', remote assignee '@janedoe'

Options:
  • Resolve interactively: ai-trackdown sync github --resolve-conflicts=prompt
  • Use local version: ai-trackdown sync github --resolve-conflicts=local
  • Use remote version: ai-trackdown sync github --resolve-conflicts=remote
```

---

## Configuration

### Configuration File Location

Default locations (in order of precedence):
1. `--config=<path>` command line option
2. `$PWD/.ai-trackdown/config.yaml` (project-specific)
3. `$HOME/.ai-trackdown/config.yaml` (user-specific)
4. `/etc/ai-trackdown/config.yaml` (system-wide)

### Configuration Schema

```yaml
# Project configuration
project:
  name: string              # Project name
  id_prefix: string         # Task ID prefix (e.g., "PROJ")
  default_epic_budget: int  # Default token budget for epics

# File format settings
formats:
  date: string              # Date format (ISO 8601)
  id_pattern: string        # ID pattern template
  
# Sync configuration
sync:
  github:
    enabled: bool
    repo: string            # "owner/repo"
    auth_method: string     # "token" | "app"
    base_url: string        # For GitHub Enterprise
    
  jira:
    enabled: bool
    url: string             # Jira instance URL
    project: string         # Jira project key
    auth_method: string     # "basic" | "oauth2"
    
  linear:
    enabled: bool
    team_id: string         # Linear team ID

# AI configuration
ai:
  llms_txt:
    auto_generate: bool     # Auto-generate on changes
    include_closed: bool    # Include closed tasks
    context_depth: int      # Related item depth
    
  token_tracking:
    enabled: bool
    require_agent_id: bool  # Require agent ID for token records
    cost_alerts: bool       # Enable cost alerts
    
  agents:
    claude:
      max_tokens_per_task: int
      allowed_operations: [string]
      
# Git integration
git:
  hooks:
    auto_install: bool      # Auto-install git hooks
    commit_parsing: bool    # Parse commit messages for task updates
    branch_protection: bool # Protect main branch
    
# UI preferences
ui:
  default_format: string    # Default output format
  color_scheme: string      # Color scheme
  date_format: string       # Human-readable date format
  
# Advanced settings
advanced:
  max_file_size: int        # Maximum task file size
  backup_enabled: bool      # Enable automatic backups
  cache_duration: string    # Cache duration for external calls
```

### Configuration Commands

#### View Current Configuration
```bash
ai-trackdown config list
```

#### Get Specific Value
```bash
ai-trackdown config get sync.github.repo
```

#### Set Value
```bash
ai-trackdown config set sync.github.repo "myorg/myrepo"
```

#### Edit Configuration
```bash
ai-trackdown config edit
```

#### Validate Configuration
```bash
ai-trackdown config validate
```

---

## Environment Variables

### Core Variables

#### `AITASKTRACK_CONFIG`
Path to configuration file
```bash
export AITASKTRACK_CONFIG=/path/to/custom/config.yaml
```

#### `AITASKTRACK_HOME`
Home directory for global settings
```bash
export AITASKTRACK_HOME=/path/to/ai-trackdown
```

#### `AITASKTRACK_LOG_LEVEL`
Logging level (debug|info|warn|error)
```bash
export AITASKTRACK_LOG_LEVEL=debug
```

### Authentication Variables

#### GitHub Integration
```bash
export GITHUB_TOKEN=ghp_xxxxxxxxxxxxxxxxxxxx
export GITHUB_ENTERPRISE_URL=https://github.company.com
```

#### Jira Integration
```bash
export JIRA_USERNAME=user@company.com
export JIRA_API_TOKEN=xxxxxxxxxxxxxxxxxxxx
export JIRA_URL=https://company.atlassian.net
```

#### Linear Integration
```bash
export LINEAR_API_KEY=lin_api_xxxxxxxxxxxxxxxxxxxx
```

### AI Service Variables

#### OpenAI/GPT
```bash
export OPENAI_API_KEY=sk-xxxxxxxxxxxxxxxxxxxx
export OPENAI_BASE_URL=https://api.openai.com/v1
```

#### Anthropic/Claude
```bash
export ANTHROPIC_API_KEY=sk-ant-xxxxxxxxxxxxxxxxxxxx
```

### Output Control Variables

#### `NO_COLOR`
Disable colored output
```bash
export NO_COLOR=1
```

#### `AITASKTRACK_FORMAT`
Default output format
```bash
export AITASKTRACK_FORMAT=json
```

#### `AITASKTRACK_EDITOR`
Default editor for interactive editing
```bash
export AITASKTRACK_EDITOR=vim
```

### Development Variables

#### `AITASKTRACK_DEBUG`
Enable debug mode
```bash
export AITASKTRACK_DEBUG=1
```

#### `AITASKTRACK_DRY_RUN`
Default to dry-run mode
```bash
export AITASKTRACK_DRY_RUN=1
```

---

## Help System

### Built-in Help

#### General Help
```bash
ai-trackdown help
ai-trackdown --help
ai-trackdown -h
```

#### Command-Specific Help
```bash
ai-trackdown help create
ai-trackdown create --help
```

#### Subcommand Help
```bash
ai-trackdown help tokens add
ai-trackdown tokens add --help
```

### Help Output Format

```
ai-trackdown create - Create new task items

USAGE:
    ai-trackdown create <type> <title> [options]

TYPES:
    epic     High-level project epic
    issue    Development issue or story  
    task     Granular implementation task

OPTIONS:
    --id=<string>          Custom ID (auto-generated if not provided)
    --assignee=<string>    Assignee (@username or email)
    --labels=<list>        Comma-separated labels
    --description=<string> Initial description
    --editor               Open in default editor
    --template=<string>    Use specific template

EPIC OPTIONS:
    --owner=<string>       Epic owner
    --target-date=<date>   Target completion date
    --token-budget=<int>   Token budget allocation

EXAMPLES:
    # Create epic
    ai-trackdown create epic "User Authentication System"
    
    # Create issue with details
    ai-trackdown create issue "Login flow" --epic=EPIC-001 --assignee=@johndoe
    
    # Create task interactively
    ai-trackdown create task --editor

For more information, visit: https://docs.ai-trackdown.com/cli/create
```

### Context-Aware Help

#### Suggest Next Commands
```bash
$ ai-trackdown create epic "Payment System"
✓ Created EPIC-002: Payment System

Suggested next steps:
  1. Add issues to epic: ai-trackdown create issue "Setup Stripe" --epic=EPIC-002
  2. Set token budget: ai-trackdown tokens budget set EPIC-002 75000
  3. Configure sync: ai-trackdown sync github --epic=EPIC-002
```

#### Error-Specific Help
```bash
$ ai-trackdown sync github
Error: GitHub integration not configured

To configure GitHub sync:
  1. Set repository: ai-trackdown config set sync.github.repo "owner/repo"
  2. Set token: export GITHUB_TOKEN=your_token
  3. Test connection: ai-trackdown sync github --dry-run
  
For detailed setup: ai-trackdown help sync github
```

---

This CLI specification provides comprehensive documentation for implementing the ai-trackdown command-line interface. The specification covers all major functionality outlined in the design document while maintaining consistency with standard CLI conventions and best practices.