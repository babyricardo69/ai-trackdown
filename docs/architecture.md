# ai-trackdown Architecture

This document provides a comprehensive overview of ai-trackdown's architecture, design principles, and implementation details.

## 🎯 Design Philosophy

ai-trackdown is built on five core principles that make it revolutionary for AI-enhanced development:

### 1. Text-First Architecture
**Principle**: All data stored as human-readable text files  
**Why**: Text is the universal interface between humans, AI agents, and tools  
**Implementation**: GitHub Flavored Markdown with YAML frontmatter  

### 2. Git-Native Storage
**Principle**: Leverage git as the primary data store  
**Why**: Distributed, versioned, and developer-familiar  
**Implementation**: Tasks live alongside code, use git workflows  

### 3. AI-Optimized Design
**Principle**: Minimize token consumption, maximize AI comprehension  
**Why**: Token costs are real and context efficiency matters  
**Implementation**: Structured formats, smart context generation, llms.txt standard  

### 4. Zero Lock-in Philosophy
**Principle**: Users own their data in portable formats  
**Why**: Avoid vendor dependency and ensure longevity  
**Implementation**: Pure markdown files, standard formats, easy migration  

### 5. Progressive Enhancement
**Principle**: Core features work offline, advanced features when connected  
**Why**: Reliability and performance over complex dependencies  
**Implementation**: Local-first with optional cloud integrations  

## 🏗️ System Architecture

### High-Level Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                     ai-trackdown System                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐         │
│  │    CLI      │    │  File       │    │   Git       │         │
│  │  Interface  │◄──►│ System      │◄──►│ Integration │         │
│  │             │    │             │    │             │         │
│  └─────────────┘    └─────────────┘    └─────────────┘         │
│         │                   │                   │              │
│         ▼                   ▼                   ▼              │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐         │
│  │   Parser    │    │  Generator  │    │    Sync     │         │
│  │   Engine    │    │   Engine    │    │   Engine    │         │
│  │             │    │             │    │             │         │
│  └─────────────┘    └─────────────┘    └─────────────┘         │
│         │                   │                   │              │
│         └───────────────────┼───────────────────┘              │
│                             ▼                                  │
│              ┌─────────────────────────────────┐               │
│              │         Data Layer              │               │
│              │                                 │               │
│              │  ┌─────┐ ┌─────┐ ┌─────┐       │               │
│              │  │Tasks│ │Epics│ │Meta │       │               │
│              │  │     │ │     │ │Data │       │               │
│              │  └─────┘ └─────┘ └─────┘       │               │
│              └─────────────────────────────────┘               │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                    External Integrations                       │
│                                                                 │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌───────────┐ │
│  │   GitHub    │ │    Jira     │ │   Linear    │ │    AI     │ │
│  │   Issues    │ │  Projects   │ │   Teams     │ │  Agents   │ │
│  └─────────────┘ └─────────────┘ └─────────────┘ └───────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

### Core Components

#### 1. CLI Interface Layer
The command-line interface provides the primary user interaction:

```
src/cli/
├── commands/           # Individual CLI commands
│   ├── create.js      # Create epics/issues/tasks
│   ├── list.js        # List and filter items
│   ├── update.js      # Update item properties
│   ├── sync.js        # External system sync
│   ├── tokens.js      # Token usage tracking
│   └── generate.js    # llms.txt generation
├── parser.js          # Command-line argument parsing
├── output.js          # Formatted terminal output
└── index.js           # Main CLI entry point
```

**Key Features:**
- Interactive prompts for complex operations
- Rich terminal output with colors and formatting
- Bash completion support
- Progress indicators for long operations

#### 2. File System Layer
Manages the markdown file structure and operations:

```
src/filesystem/
├── structure.js       # Directory structure management
├── templates.js       # File template system
├── validation.js      # File format validation
├── indexing.js        # File indexing and search
└── migration.js       # Format migration utilities
```

**File Format Specification:**
```markdown
---
# YAML frontmatter with structured metadata
id: ISSUE-001
type: issue
title: "Task title"
status: in-progress
epic: EPIC-001
assignee: @username
created: 2025-01-07T10:00:00Z
updated: 2025-01-07T14:30:00Z
labels: [tag1, tag2]
estimate: 5
token_usage:
  total: 1247
  by_agent:
    claude: 892
    gpt4: 355
sync:
  github: 145
  jira: AUTH-234
---

# Markdown content
Task description, acceptance criteria, technical notes, etc.
```

#### 3. Parser Engine
Handles markdown parsing and frontmatter processing:

```
src/parsers/
├── markdown.js        # Core markdown parsing
├── frontmatter.js     # YAML frontmatter handling
├── validation.js      # Schema validation
├── transform.js       # Data transformation
└── export.js          # Export to various formats
```

**Parsing Pipeline:**
1. **File Reading**: Read markdown file from filesystem
2. **Frontmatter Extraction**: Parse YAML metadata
3. **Content Parsing**: Process markdown body
4. **Validation**: Validate against schema
5. **Transformation**: Convert to internal format

#### 4. Generator Engine
Creates llms.txt files and other generated content:

```
src/generators/
├── llms-txt.js        # llms.txt standard implementation
├── context.js         # AI context generation
├── reports.js         # Status and analytics reports
├── exports.js         # Data export utilities
└── hooks.js           # Git hook generation
```

**llms.txt Generation Process:**
```javascript
// Simplified generation flow
const generateLlmsTxt = async (project) => {
  const overview = await generateProjectOverview(project);
  const keyFiles = await identifyKeyFiles(project);
  const currentFocus = await getCurrentPriorities(project);
  const quickCommands = generateQuickCommands(project);
  const integrationStatus = await getIntegrationStatus(project);
  
  return formatLlmsTxt({
    overview,
    keyFiles,
    currentFocus,
    quickCommands,
    integrationStatus
  });
};
```

#### 5. Sync Engine
Manages bidirectional synchronization with external systems:

```
src/sync/
├── base.js            # Base synchronization interface
├── github.js          # GitHub Issues integration
├── jira.js            # Jira integration
├── linear.js          # Linear integration
├── conflict.js        # Conflict resolution
└── mapping.js         # Field mapping utilities
```

**Synchronization Architecture:**
```
Local Tasks (Source of Truth)
      ↕ (bidirectional sync)
External Systems (GitHub/Jira/Linear)
```

**Sync Strategy:**
1. **Local First**: Local markdown files are the source of truth
2. **Incremental Sync**: Only sync changed items via timestamps
3. **Conflict Resolution**: Last-write-wins with conflict files
4. **Field Mapping**: Configurable field mappings per platform

#### 6. Git Integration
Seamlessly integrates with git workflows:

```
src/git/
├── hooks.js           # Git hook management
├── commits.js         # Commit message parsing
├── branches.js        # Branch naming utilities
├── status.js          # Git status integration
└── history.js         # Git history analysis
```

**Git Hook System:**
```bash
# post-commit hook
#!/bin/bash
# Auto-update task status based on commit messages
ai-trackdown parse-commit "$1"
ai-trackdown generate-llms-txt

# pre-push hook  
#!/bin/bash
# Sync with external systems before push
ai-trackdown sync --all
ai-trackdown validate-tokens --warn-threshold=1000
```

## 📁 Directory Structure Deep Dive

### Project Root Structure
```
project-root/
├── .ai-trackdown/              # System configuration and metadata
│   ├── config.yaml           # Project configuration
│   ├── cache/                # Performance caches
│   │   ├── index.json        # File index cache
│   │   └── tokens.json       # Token usage cache
│   ├── sync/                 # Synchronization state
│   │   ├── github.json       # GitHub sync metadata
│   │   ├── jira.json         # Jira sync metadata
│   │   └── linear.json       # Linear sync metadata
│   ├── templates/            # Custom templates (optional)
│   │   ├── epic.md          
│   │   ├── issue.md         
│   │   └── task.md          
│   └── llms.txt             # Auto-generated AI index
├── tasks/                    # Task storage hierarchy
│   ├── epics/               # High-level project epics
│   │   ├── EPIC-001-user-authentication.md
│   │   └── EPIC-002-payment-integration.md
│   ├── issues/              # Development issues/features
│   │   ├── ISSUE-001-login-flow.md
│   │   ├── ISSUE-002-password-reset.md
│   │   └── ISSUE-003-oauth-integration.md
│   └── tasks/               # Granular implementation tasks
│       ├── TASK-001-create-login-form.md
│       ├── TASK-002-implement-oauth.md
│       └── TASK-003-add-jwt-handling.md
├── docs/                    # Extended documentation
│   ├── llms-full.txt        # Complete project context for AI
│   ├── architecture.md      # Project architecture notes
│   └── decisions.md         # Architectural decision records
└── TASKTRACK.md             # Project dashboard/overview
```

### Configuration System

#### Primary Configuration (.ai-trackdown/config.yaml)
```yaml
version: 1
project:
  name: "My AI Project"
  id_prefix: "PROJ"
  default_epic_budget: 50000
  timezone: "UTC"

formats:
  date: "YYYY-MM-DDTHH:mm:ssZ"
  id_pattern: "{type}-{number:04d}"
  branch_pattern: "{type}/{id}-{slug}"

sync:
  github:
    enabled: true
    repo: "org/repo-name"
    auth_method: "token"
    sync_interval: "1h"
    field_mapping:
      title: title
      body: description
      labels: labels
      assignee: assignee
      
  jira:
    enabled: true
    url: "https://company.atlassian.net"
    project: "PROJ"
    auth_method: "oauth2"
    
  linear:
    enabled: false

ai:
  llms_txt:
    auto_generate: true
    include_closed: false
    context_depth: 3
    update_frequency: "on_change"
    
  token_tracking:
    enabled: true
    require_agent_id: true
    cost_alerts: true
    providers:
      claude:
        model: "claude-3.5-sonnet"
        cost_per_1k_input: 0.003
        cost_per_1k_output: 0.015
      gpt4:
        model: "gpt-4-turbo"
        cost_per_1k_input: 0.01
        cost_per_1k_output: 0.03
        
  agents:
    claude:
      max_tokens_per_task: 5000
      allowed_operations: ["read", "comment", "update"]
      context_format: "detailed"
    
git:
  hooks:
    auto_install: true
    commit_parsing: true
    branch_protection: false
  
  patterns:
    commit_task_ref: "\\b(EPIC|ISSUE|TASK)-\\d+\\b"
    status_keywords:
      in_progress: ["start", "begin", "wip"]
      in_review: ["review", "pr", "merge"]
      done: ["fix", "close", "resolve", "complete"]

workflows:
  default:
    statuses: ["todo", "in-progress", "in-review", "done"]
    transitions:
      todo -> in-progress: auto
      in-progress -> in-review: requires_commit
      in-review -> done: requires_approval
      
automation:
  rules:
    - trigger: "commit"
      pattern: "fix\\((.+)\\):"
      action: "update_status"
      status: "in-review"
      
    - trigger: "token_usage"
      threshold: 0.9
      action: "alert"
      channels: ["slack", "email"]
```

## 🔄 Data Flow Architecture

### 1. Task Creation Flow
```
User Input → CLI Parser → Template Engine → File Writer → Git Add → Hook Trigger → llms.txt Update
```

**Detailed Steps:**
1. **User Input**: `ai-trackdown create issue "Login flow"`
2. **CLI Parser**: Parse command and extract parameters
3. **Template Engine**: Load appropriate template (issue.md)
4. **File Writer**: Create markdown file with frontmatter
5. **Git Add**: Stage new file in git
6. **Hook Trigger**: Run post-commit hooks
7. **llms.txt Update**: Regenerate AI context files

### 2. Status Update Flow
```
Status Change → File Update → Git Commit → External Sync → Notification
```

**Implementation:**
```javascript
const updateTaskStatus = async (taskId, newStatus) => {
  // 1. Load and validate task
  const task = await loadTask(taskId);
  validateStatusTransition(task.status, newStatus);
  
  // 2. Update file
  task.status = newStatus;
  task.updated = new Date().toISOString();
  await saveTask(task);
  
  // 3. Git operations
  await gitAdd(task.filepath);
  await gitCommit(`update(${taskId}): status changed to ${newStatus}`);
  
  // 4. External sync
  if (config.sync.enabled) {
    await syncWithExternal(task);
  }
  
  // 5. Regenerate AI context
  await generateLlmsTxt();
  
  // 6. Notifications
  await notifyStatusChange(task, newStatus);
};
```

### 3. Synchronization Flow
```
External Change → Webhook/Poll → Conflict Check → Merge → Local Update → Git Commit
```

**Bidirectional Sync Strategy:**
```javascript
const syncWithGitHub = async (options = {}) => {
  const { since, full } = options;
  
  // 1. Get local changes
  const localChanges = await getLocalChanges(since);
  
  // 2. Get remote changes  
  const remoteChanges = await github.getChanges(since);
  
  // 3. Identify conflicts
  const conflicts = identifyConflicts(localChanges, remoteChanges);
  
  // 4. Resolve conflicts (last-write-wins)
  const resolved = await resolveConflicts(conflicts);
  
  // 5. Apply remote changes locally
  for (const change of remoteChanges) {
    await applyRemoteChange(change);
  }
  
  // 6. Push local changes remotely
  for (const change of localChanges) {
    await pushLocalChange(change);
  }
  
  // 7. Update sync metadata
  await updateSyncState('github', new Date());
};
```

### 4. Token Tracking Flow
```
AI Operation → Token Count → Attribution → File Update → Budget Check → Alert (if needed)
```

**Token Attribution System:**
```javascript
const trackTokenUsage = async (taskId, agent, tokenCount, operation) => {
  // 1. Load task
  const task = await loadTask(taskId);
  
  // 2. Update token usage
  if (!task.token_usage) task.token_usage = { total: 0, by_agent: {} };
  task.token_usage.total += tokenCount;
  task.token_usage.by_agent[agent] = (task.token_usage.by_agent[agent] || 0) + tokenCount;
  
  // 3. Calculate cost
  const cost = calculateCost(agent, tokenCount);
  
  // 4. Update epic budget
  if (task.epic) {
    await updateEpicBudget(task.epic, tokenCount, cost);
  }
  
  // 5. Check budget alerts
  const budgetStatus = await checkBudgetStatus(task.epic);
  if (budgetStatus.alert) {
    await sendBudgetAlert(budgetStatus);
  }
  
  // 6. Save changes
  await saveTask(task);
  
  // 7. Log activity
  await logTokenUsage(taskId, agent, tokenCount, operation, cost);
};
```

## 🧠 AI Integration Architecture

### llms.txt Standard Implementation

ai-trackdown implements the llms.txt standard for AI discoverability:

#### Index File Structure (/llms.txt)
```txt
# ai-trackdown Project Index
# Generated: 2025-01-07T15:00:00Z
# Format: llms.txt v1.0

## Project Overview
Task management for [Project Name]
Technology: Node.js, React, PostgreSQL
Team size: 5 developers
Active epics: 3
Open issues: 27
Current sprint: Authentication system

## Key Files
/tasks/epics/: High-level project epics and goals
/tasks/issues/: Active development issues and features  
/tasks/tasks/: Granular implementation tasks
/TASKTRACK.md: Project status dashboard and overview
/docs/llms-full.txt: Complete project context and history

## Current Focus  
Primary: EPIC-001 User Authentication (70% complete)
- ISSUE-001: OAuth2 login flow (in-progress, @alice)
- ISSUE-002: Password reset system (pending)
- ISSUE-003: Two-factor authentication (blocked)

Secondary: EPIC-002 Payment Integration (planning)
- Requirements gathering in progress
- Stripe vs PayPal evaluation

## Quick Commands
View open issues: `ai-trackdown list --status=open --assignee=@me`
Check token usage: `ai-trackdown tokens report --period=week`
Generate context: `ai-trackdown context EPIC-001 --for=claude`
Sync with GitHub: `ai-trackdown sync github --incremental`

## Integration Status
GitHub Issues: Synced (2025-01-07T14:55:00Z)
Jira: Synced (2025-01-07T14:00:00Z)  
Linear: Not configured

## Token Budget Status
Total budget: 100,000 tokens/month
Used this month: 25,847 tokens (25.8%)
Largest consumer: EPIC-001 (12,847 tokens)
Efficiency trend: Improving (+15% vs last month)
```

#### Complete Context File (/docs/llms-full.txt)
```txt
# ai-trackdown Complete Project Context
# Generated: 2025-01-07T15:00:00Z
# This file provides comprehensive context for AI agents

## Project Architecture
[Detailed architecture overview]

## Active Work Items
[Complete task details with context]

## Code Structure  
[Key files and their purposes]

## Recent Decisions
[Architectural decisions and rationale]

## Development Standards
[Coding standards, patterns, best practices]

## Integration Points
[API endpoints, external dependencies]

## Testing Strategy
[Test coverage, automation, quality gates]
```

### AI Context Optimization

#### Context Markers in Tasks
```markdown
## AI Context
<!-- AI_CONTEXT_START -->
This task implements OAuth2 authentication for the user management system.

Key Files:
- src/auth/oauth.js: Main OAuth2 implementation
- src/components/OAuthButton.tsx: Frontend OAuth button
- config/oauth.json: Provider configurations

Dependencies:
- passport-oauth2: OAuth2 strategy implementation
- jsonwebtoken: JWT token handling
- express-session: Session management

Security Considerations:
- PKCE flow for enhanced security
- CSRF protection via state parameter
- Secure cookie handling with httpOnly flag

Related Context:
- EPIC-001: Parent authentication epic
- ISSUE-002: Password reset (shares user model)
- TASK-001: User model creation (dependency)

Business Logic:
- Support Google and GitHub providers
- Automatic user profile creation
- Role assignment based on email domain
<!-- AI_CONTEXT_END -->
```

#### Token-Optimized Context Generation
```javascript
const generateOptimizedContext = (task, options = {}) => {
  const { depth = 1, includeHistory = false, targetAgent = 'claude' } = options;
  
  let context = {
    task: sanitizeForTokens(task),
    dependencies: [],
    codeContext: {},
    businessContext: {}
  };
  
  // Include dependencies based on depth
  if (depth > 0) {
    context.dependencies = getRelatedTasks(task, depth);
  }
  
  // Agent-specific formatting
  if (targetAgent === 'claude') {
    context = formatForClaude(context);
  } else if (targetAgent === 'gpt4') {
    context = formatForGPT4(context);
  }
  
  // Token budget management
  const estimatedTokens = estimateTokenCount(context);
  if (estimatedTokens > options.maxTokens) {
    context = truncateContext(context, options.maxTokens);
  }
  
  return context;
};
```

### Token Economics System

#### Cost Tracking Model
```javascript
const tokenProviders = {
  claude: {
    model: 'claude-3.5-sonnet',
    costPer1kInput: 0.003,
    costPer1kOutput: 0.015,
    estimateRatio: 0.7 // input/output ratio
  },
  gpt4: {
    model: 'gpt-4-turbo',
    costPer1kInput: 0.01,
    costPer1kOutput: 0.03,
    estimateRatio: 0.6
  },
  copilot: {
    model: 'gpt-3.5-turbo',
    costPer1kInput: 0.0015,
    costPer1kOutput: 0.002,
    estimateRatio: 0.8
  }
};

const calculateTokenCost = (provider, inputTokens, outputTokens) => {
  const config = tokenProviders[provider];
  const inputCost = (inputTokens / 1000) * config.costPer1kInput;
  const outputCost = (outputTokens / 1000) * config.costPer1kOutput;
  return inputCost + outputCost;
};
```

#### Budget Management
```javascript
const budgetManager = {
  async checkBudget(epicId, proposedUsage) {
    const epic = await loadEpic(epicId);
    const currentUsage = epic.token_usage?.total || 0;
    const budget = epic.token_budget || config.defaultEpicBudget;
    
    const newUsage = currentUsage + proposedUsage;
    const utilizationRate = newUsage / budget;
    
    return {
      budget,
      currentUsage,
      proposedUsage,
      newUsage,
      utilizationRate,
      withinBudget: utilizationRate <= 1.0,
      nearLimit: utilizationRate > 0.8,
      alertNeeded: utilizationRate > config.budgetAlertThreshold
    };
  },
  
  async optimizeContext(context, maxTokens) {
    // AI-driven context optimization
    const tokenCount = estimateTokenCount(context);
    
    if (tokenCount <= maxTokens) return context;
    
    // Priority-based truncation
    const optimized = {
      ...context,
      dependencies: context.dependencies.slice(0, 3), // Keep top 3
      history: [], // Remove history first
      codeContext: summarizeCodeContext(context.codeContext)
    };
    
    return optimized;
  }
};
```

## 🔧 Plugin Architecture

### Plugin System Design
```
src/plugins/
├── base.js            # Base plugin interface
├── registry.js        # Plugin registry and loader
├── hooks.js           # Plugin hook system
└── builtin/           # Built-in plugins
    ├── github.js      # GitHub integration
    ├── jira.js        # Jira integration
    └── slack.js       # Slack notifications
```

### Plugin Interface
```javascript
class PluginBase {
  constructor(config) {
    this.config = config;
    this.name = this.constructor.name;
  }
  
  // Lifecycle hooks
  async initialize() {}
  async activate() {}
  async deactivate() {}
  
  // Event hooks
  async onTaskCreated(task) {}
  async onTaskUpdated(task, changes) {}
  async onTokenUsage(task, usage) {}
  async onSync(event) {}
  
  // CLI extensions
  getCommands() { return []; }
  getMiddleware() { return []; }
}
```

### Example Plugin (Slack Notifications)
```javascript
class SlackPlugin extends PluginBase {
  async onTaskUpdated(task, changes) {
    if (changes.status && this.config.notifyOnStatusChange) {
      await this.sendSlackMessage({
        channel: this.config.channel,
        message: `Task ${task.id} status changed to ${task.status}`,
        task: task
      });
    }
  }
  
  async onTokenUsage(task, usage) {
    if (usage.budget_alert) {
      await this.sendSlackMessage({
        channel: this.config.alertChannel,
        message: `⚠️ Token budget alert for ${task.epic}: ${usage.utilization}% used`,
        priority: 'high'
      });
    }
  }
}
```

## 📊 Performance Considerations

### File System Optimization
- **Lazy Loading**: Only load task files when needed
- **Indexing**: Maintain searchable index cache
- **Batch Operations**: Group file operations for efficiency
- **Watch System**: Monitor file changes for real-time updates

### Memory Management
- **Streaming**: Process large files in streams
- **Caching**: LRU cache for frequently accessed tasks
- **Garbage Collection**: Explicit cleanup of large objects
- **Memory Limits**: Configurable memory usage limits

### Network Optimization
- **Connection Pooling**: Reuse HTTP connections
- **Rate Limiting**: Respect external API limits
- **Retry Logic**: Exponential backoff for failures
- **Compression**: Gzip compression for API calls

### Scalability Targets
- **Tasks**: Support 10,000+ tasks per repository
- **Performance**: < 100ms for read operations
- **Sync**: < 500ms for incremental sync
- **Context**: < 1s for llms.txt generation

## 🔒 Security Architecture

### Data Protection
- **Local Storage**: No sensitive data in plain text
- **Encryption**: Optional encryption for sensitive fields
- **Credentials**: OS keychain integration
- **Audit Trail**: Complete activity logging

### API Security
- **Authentication**: OAuth2 and token-based auth
- **Authorization**: Scope-limited API access
- **Rate Limiting**: Prevent API abuse
- **Validation**: Input sanitization and validation

### Git Security
- **Signed Commits**: GPG signing support
- **Branch Protection**: Prevent direct pushes to main
- **Hook Validation**: Secure git hook execution
- **Secret Detection**: Prevent accidental secret commits

## 🚀 Future Architecture Considerations

### Planned Enhancements
1. **Distributed Sync**: Multi-repository coordination
2. **Real-time Collaboration**: WebSocket-based live updates
3. **AI Agent Framework**: Pluggable AI agent system
4. **Advanced Analytics**: Machine learning insights
5. **Mobile Support**: Offline-capable mobile apps

### Extensibility Points
1. **Custom Sync Providers**: New integration platforms
2. **AI Model Support**: Additional LLM providers
3. **Workflow Engines**: Custom workflow implementations
4. **Report Generators**: Custom analytics and reports
5. **Authentication Providers**: Enterprise SSO integration

This architecture ensures ai-trackdown remains lightweight, performant, and extensible while providing revolutionary capabilities for AI-enhanced development teams.