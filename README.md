# ai-trackdown

> **Revolutionary markdown-based task management for the AI development era**

[![npm version](https://badge.fury.io/js/ai-trackdown.svg)](https://badge.fury.io/js/ai-trackdown)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Node.js CI](https://github.com/ai-trackdown/ai-trackdown/workflows/Node.js%20CI/badge.svg)](https://github.com/ai-trackdown/ai-trackdown/actions)

## 🚨 Why AI-Trackdown is the issue management system for the ai orchestrated development era

**The Problem**: Current task management systems were designed for human-only workflows. In the AI development era, we face:

- **Token Blindness**: No visibility into AI assistant costs across projects
- **Context Fragmentation**: AI agents lose context between sessions, requiring expensive re-explanations
- **Integration Overhead**: Complex APIs and databases that slow down development velocity
- **Vendor Lock-in**: Proprietary systems that trap your project data
- **AI Accessibility**: Traditional systems are opaque to AI agents, requiring complex integrations

**The Solution**: ai-trackdown is the first task management system designed from the ground up for human-AI collaborative development:

✅ **Native AI Context**: Built-in llms.txt support means AI agents understand your project instantly  
✅ **Token Economics**: Track and optimize AI costs at every level (task/epic/project)  
✅ **Git-Native**: Your tasks live where your code lives - no separate systems  
✅ **Zero Lock-in**: Pure markdown files you can read, edit, and migrate anywhere  
✅ **AI-First Design**: Optimized for minimal token consumption and maximum AI efficiency  

ai-trackdown isn't just another task manager - it's the missing infrastructure layer for AI-enhanced development teams.

## 🚀 Key Features

- **Pure Markdown Storage**: All data stored as human-readable markdown files using GitHub Flavored Markdown (GFM)
- **Native llms.txt Support**: Automatic generation of llms.txt files for AI discoverability and context
- **Git-Native**: Leverages git's distributed nature with automatic state management via hooks
- **Token Usage Tracking**: Track AI token usage at task/epic/project levels with cost analysis
- **Bidirectional Sync**: Sync with GitHub Issues, Jira, and Linear in both directions
- **Zero Dependencies**: Core functionality works offline with no external dependencies
- **AI-Optimized**: Structured for minimal token consumption and maximum AI efficiency

## 📦 Installation

### Global Installation (Recommended)

```bash
npm install -g ai-trackdown
```

### Local Installation

```bash
npm install ai-trackdown
```

### From Source

```bash
git clone https://github.com/ai-trackdown/ai-trackdown.git
cd ai-trackdown
npm install
npm link
```

## 🏁 Quick Start

### 1. Initialize a New Project

```bash
cd your-project
ai-trackdown init
```

This creates the basic directory structure:
```
your-project/
├── .ai-trackdown/
│   ├── config.yaml
│   └── llms.txt
├── tasks/
│   ├── epics/
│   ├── issues/
│   └── tasks/
└── TASKTRACK.md
```

### 2. Create Your First Epic

```bash
ai-trackdown create epic "User Authentication System"
```

### 3. Add Issues to the Epic

```bash
ai-trackdown create issue "Implement login flow" --epic=EPIC-001
ai-trackdown create issue "Add password reset" --epic=EPIC-001
```

### 4. Break Down Issues into Tasks

```bash
ai-trackdown create task "Create login form component" --issue=ISSUE-001
ai-trackdown create task "Implement OAuth2 integration" --issue=ISSUE-001
```

### 5. Track AI Token Usage

```bash
ai-trackdown tokens add ISSUE-001 --agent=claude --count=892 --purpose="Requirements analysis"
```

### 6. Generate llms.txt for AI Context

```bash
ai-trackdown generate llms-txt
```

## 🔧 Configuration

### Basic Configuration

Create `.ai-trackdown/config.yaml`:

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

### Advanced Configuration

See [Configuration Guide](docs/configuration.md) for complete options including:
- Integration setups (GitHub, Jira, Linear)
- Token tracking configuration
- AI agent permissions
- Git hooks setup

## 📖 Usage

### Core Commands

```bash
# Project Management
ai-trackdown init                                    # Initialize new project
ai-trackdown status                                  # Show project status

# Creating Items
ai-trackdown create epic "Epic Title"
ai-trackdown create issue "Issue Title" --epic=EPIC-001
ai-trackdown create task "Task Title" --issue=ISSUE-001

# Listing and Filtering
ai-trackdown list                                    # List all items
ai-trackdown list --type=issue --status=open        # Filter by type and status
ai-trackdown list --assignee=@me                    # Show your assignments
ai-trackdown list --epic=EPIC-001                   # Show items in specific epic

# Updating Status
ai-trackdown update ISSUE-001 --status=in-progress
ai-trackdown close TASK-001 --comment="Completed"
ai-trackdown assign ISSUE-001 @username

# Token Tracking
ai-trackdown tokens add ISSUE-001 --agent=claude --count=500
ai-trackdown tokens report --period=week
ai-trackdown tokens budget --epic=EPIC-001 --limit=50000

# Sync Operations
ai-trackdown sync github --full                     # Full sync with GitHub
ai-trackdown sync jira --since=1h                   # Incremental sync
ai-trackdown sync status                             # Show sync status

# AI Operations
ai-trackdown generate llms-txt                       # Generate llms.txt index
ai-trackdown context EPIC-001                        # Get AI context for epic
ai-trackdown analyze ISSUE-001 --suggest-tasks      # AI task suggestions
```

### File Structure

ai-trackdown uses a simple, intuitive file structure:

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

### Task File Format

Each task file follows a structured markdown format:

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

## Related Tasks
- Blocks: TASK-003, TASK-004
- Blocked by: ISSUE-002
- Related: ISSUE-005
```

## 🤖 AI Integration

### llms.txt Support

ai-trackdown automatically generates llms.txt files following the standard:

- `/llms.txt` - Project index for quick AI context
- `/docs/llms-full.txt` - Complete project context

### Token Tracking

Track AI token usage across your project:

```bash
# Add token usage
ai-trackdown tokens add ISSUE-001 --agent=claude --count=892

# View reports
ai-trackdown tokens report --by=epic --period=month

# Set budgets
ai-trackdown tokens budget --epic=EPIC-001 --limit=50000
```

### AI Context Optimization

ai-trackdown optimizes context for AI agents:
- Automatic context markers in tasks
- Smart token budgeting
- Efficient context generation
- Agent-specific permissions

## 🔄 Integrations

### GitHub Issues

```bash
# Setup GitHub sync
ai-trackdown sync setup github --repo=org/repo --token=your-token

# Sync operations
ai-trackdown sync github --full
ai-trackdown sync github --since=1h
```

### Jira

```bash
# Setup Jira sync
ai-trackdown sync setup jira --url=https://company.atlassian.net --project=PROJ

# Sync operations
ai-trackdown sync jira --full
```

### Linear

```bash
# Setup Linear sync
ai-trackdown sync setup linear --team=engineering --token=your-token

# Sync operations
ai-trackdown sync linear --full
```

## 🛠️ Development

### Prerequisites

- Node.js 18.0.0 or higher
- npm or yarn
- Git

### Setup

```bash
git clone https://github.com/ai-trackdown/ai-trackdown.git
cd ai-trackdown
npm install
npm run dev
```

### Testing

```bash
npm test                    # Run all tests
npm run test:watch         # Run tests in watch mode
npm run test:coverage      # Run tests with coverage
```

### Building

```bash
npm run build              # Build for production
npm run lint               # Run linting
```

## 📚 Comprehensive Documentation

### 🚀 Getting Started
- **[Getting Started Guide](docs/getting-started.md)** - Complete 5-minute setup tutorial with examples
- **[FAQ](docs/faq.md)** - Answers to common questions about ai-trackdown's revolutionary approach

### 🏗️ Technical Deep Dive  
- **[Architecture Guide](docs/architecture.md)** - System design, AI integration patterns, and scaling strategies
- **[API Reference](docs/api-reference.md)** - Complete CLI command reference with examples
- **[Configuration Guide](docs/configuration.md)** - Advanced configuration options and team setups

### 🔄 Platform Integration
- **[Integration Guide](docs/integrations.md)** - Setup GitHub, Jira, Linear sync with field mapping examples
- **[Best Practices](docs/best-practices.md)** - Proven strategies for AI-human collaboration workflows

### 💡 Examples and Templates
- **[Basic Setup Example](examples/basic-setup/)** - Small team starter configuration
- **[Enterprise Integration](examples/enterprise-integration/)** - Large organization setup with multi-team coordination
- **[Sample Tasks](examples/sample-tasks/)** - Real-world epic, issue, and task examples
- **[Configuration Templates](examples/configs/)** - Ready-to-use config files for different scenarios

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

### Quick Contributing Steps

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## 📋 Roadmap

### Phase 1: Core System ✅
- [x] Basic file structure and CLI
- [x] Git hooks implementation
- [x] Markdown parsing and validation
- [x] Simple status management

### Phase 2: AI Integration 🚧
- [x] llms.txt generation
- [x] Token tracking system
- [x] AI context markers
- [ ] Claude/GPT optimization

### Phase 3: Sync Capabilities 📋
- [ ] GitHub Issues bidirectional sync
- [ ] Jira integration with webhooks
- [ ] Linear API integration
- [ ] Conflict resolution system

### Phase 4: Advanced Features 📋
- [ ] Web UI (optional)
- [ ] Analytics dashboard
- [ ] Multi-project support
- [ ] Plugin system

## 📄 License

MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [llms.txt standard](https://llmstxt.org/) for AI context conventions
- [GitHub Flavored Markdown](https://github.github.com/gfm/) for file format
- The open-source community for inspiration and best practices

## 💬 Support

- [GitHub Issues](https://github.com/ai-trackdown/ai-trackdown/issues)
- [Discussions](https://github.com/ai-trackdown/ai-trackdown/discussions)
- [Documentation](https://ai-trackdown.github.io/ai-trackdown/)

---

**Made with ❤️ for human-AI collaborative development**