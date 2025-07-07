# AI Track Down

> **Revolutionary markdown-based task management framework for the AI development era**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Documentation](https://img.shields.io/badge/docs-framework-blue.svg)](https://github.com/ai-trackdown/ai-trackdown)

## 🚨 Why AI Track Down is the issue management system for the ai orchestrated development era

**The Problem**: Current task management systems were designed for human-only workflows. In the AI development era, we face:

- **Token Blindness**: No visibility into AI assistant costs across projects
- **Context Fragmentation**: AI agents lose context between sessions, requiring expensive re-explanations
- **Integration Overhead**: Complex APIs and databases that slow down development velocity
- **Vendor Lock-in**: Proprietary systems that trap your project data
- **AI Accessibility**: Traditional systems are opaque to AI agents, requiring complex integrations

**The Solution**: AI Track Down is the first task management framework designed from the ground up for human-AI collaborative development:

✅ **Native AI Context**: Built-in llms.txt support means AI agents understand your project instantly  
✅ **Token Economics**: Track and optimize AI costs at every level (task/epic/project)  
✅ **Git-Native**: Your tasks live where your code lives - no separate systems  
✅ **Zero Lock-in**: Pure markdown files you can read, edit, and migrate anywhere  
✅ **AI-First Design**: Optimized for minimal token consumption and maximum AI efficiency  

AI Track Down isn't just another task manager - it's the missing documentation framework for AI-enhanced development teams.

> **📋 Documentation Framework**: This project provides templates, examples, and documentation patterns. Working CLI tools are being developed separately.

## 🚀 Key Features

- **Pure Markdown Templates**: All templates and examples use human-readable markdown with GitHub Flavored Markdown (GFM)
- **Native llms.txt Support**: Templates for generating llms.txt files for AI discoverability and context
- **Git-Native Structure**: File organization leverages git's distributed nature with state management patterns
- **Token Usage Tracking**: Template structures for tracking AI token usage at task/epic/project levels
- **Integration Patterns**: Documentation patterns for GitHub Issues, Jira, and Linear integration
- **Zero Dependencies**: Template system works offline with no external dependencies
- **AI-Optimized**: Structured for minimal token consumption and maximum AI efficiency

## 📦 Getting Started

### Clone the Framework

```bash
git clone https://github.com/ai-trackdown/ai-trackdown.git
cd ai-trackdown
```

### Copy Templates to Your Project

```bash
# Copy the template structure to your project
cp -r templates/ your-project/
cd your-project
```

### Framework Structure

This framework provides documentation patterns and templates - no installation required.

## 🤖 AI Agent Setup

### For AI Agents Working with This Framework

If you're an AI agent helping with AI Track Down implementation, read the complete setup instructions in:

**[AI-AGENT-PROMPT.md](AI-AGENT-PROMPT.md)** - Complete system prompt and workflow guide

This file contains:
- Complete framework context and structure
- Task creation and updating workflows  
- Token tracking guidelines and best practices
- AI-optimized templates and patterns
- Integration with development workflows

**Quick AI Agent Workflow:**
1. Read `AI-AGENT-PROMPT.md` for complete context
2. Copy templates from `/templates/` directory for new tasks
3. Update task status and token usage in YAML frontmatter
4. Use AI context markers for technical implementation details
5. Update project dashboard in `AI-TRACKDOWN.md`

This framework is optimized for AI-human collaboration with built-in token tracking and context management.

## 🏁 Quick Start

### 1. Set Up Your Project Structure

Copy the template directory structure to your project:

```bash
# Copy templates from the framework
cp -r templates/basic-setup/ your-project/
cd your-project
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
└── AI-TRACKDOWN.md
```

### 2. Create Your First Epic

Copy and customize the epic template:

```bash
# Use the epic template
cp templates/task-templates/epic-template.md tasks/epics/EPIC-001-user-authentication.md
# Edit with your epic details
```

### 3. Add Issues to the Epic

Copy and customize issue templates:

```bash
# Use the issue template
cp templates/task-templates/issue-template.md tasks/issues/ISSUE-001-login-flow.md
# Edit with your issue details and link to EPIC-001
```

### 4. Break Down Issues into Tasks

Copy and customize task templates:

```bash
# Use the task template
cp templates/task-templates/task-template.md tasks/tasks/TASK-001-login-form.md
# Edit with your task details and link to ISSUE-001
```

### 5. Track AI Token Usage

Follow the token tracking patterns in the templates to manually log AI usage.

### 6. Generate llms.txt for AI Context

Use the llms.txt template and customize for your project structure.

## 🔧 Configuration

### Basic Configuration

Use the provided `.ai-trackdown/config.yaml` template:

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

See [Configuration Guide](docs/configuration.md) for complete template options including:
- Integration setup patterns (GitHub, Jira, Linear)
- Token tracking configuration examples
- AI agent permission templates
- Git hooks setup guides

## 📖 Usage

### Framework Workflow

Follow these manual workflows using the provided templates:

```bash
# Project Setup
cp -r templates/basic-setup/ your-project/           # Copy project structure
vim your-project/.ai-trackdown/config.yaml          # Customize configuration

# Creating Items
cp templates/task-templates/epic-template.md tasks/epics/EPIC-XXX.md    # Create epic
cp templates/task-templates/issue-template.md tasks/issues/ISSUE-XXX.md # Create issue  
cp templates/task-templates/task-template.md tasks/tasks/TASK-XXX.md    # Create task

# Managing Status
# Edit YAML frontmatter in task files to update status
# Use examples/status-workflows/ for common patterns

# Token Tracking
# Follow token tracking patterns in task templates
# Use examples/token-tracking/ for reporting templates

# Integration Setup
# Follow examples/integrations/ for GitHub/Jira/Linear patterns
# Use webhook templates for bidirectional sync

# AI Context Generation
# Use templates/llms-txt/ for generating AI context files
# Follow examples/ai-integration/ for optimization patterns
```

### File Structure

The framework provides this simple, intuitive template structure:

```
project-root/
├── .ai-trackdown/
│   ├── config.yaml              # Configuration template
│   ├── sync/                    # Sync pattern templates
│   │   ├── github.json
│   │   ├── jira.json
│   │   └── linear.json
│   └── llms.txt                 # AI context template
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
│   └── llms-full.txt            # Complete task context template
└── AI-TRACKDOWN.md                 # Project task overview template
```

### Task File Format

Each task template follows a structured markdown format:

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

The framework provides llms.txt templates following the standard:

- `/llms.txt` - Project index template for quick AI context
- `/docs/llms-full.txt` - Complete project context template

### Token Tracking

Track AI token usage using the provided templates:

```bash
# Follow token tracking patterns in task templates
# Edit YAML frontmatter to log token usage:
#   token_usage:
#     total: 892
#     by_agent:
#       claude: 892

# Use examples/token-tracking/ for reporting templates
# Customize budget tracking patterns for your workflow
```

### AI Context Optimization

The framework optimizes context for AI agents through:
- Context markers in task templates
- Token budgeting patterns
- Efficient context generation templates
- Agent-specific permission examples

## 🔄 Integrations

### GitHub Issues

```bash
# Follow GitHub integration patterns in examples/integrations/github/
# Use webhook templates for bidirectional sync
# Configure .ai-trackdown/sync/github.json using provided templates
```

### Jira

```bash
# Follow Jira integration patterns in examples/integrations/jira/
# Use API mapping templates for field synchronization
# Configure .ai-trackdown/sync/jira.json using provided templates
```

### Linear

```bash
# Follow Linear integration patterns in examples/integrations/linear/
# Use GraphQL templates for API integration
# Configure .ai-trackdown/sync/linear.json using provided templates
```

## 🛠️ Framework Development

### Prerequisites

- Git for version control
- Text editor for markdown editing
- Basic understanding of YAML and markdown

### Contributing to Templates

```bash
git clone https://github.com/ai-trackdown/ai-trackdown.git
cd ai-trackdown
# Edit templates in templates/ directory
# Add examples in examples/ directory
# Update documentation as needed
```

### Template Validation

```bash
# Validate YAML frontmatter in templates
# Check markdown formatting
# Ensure example consistency
# Test template copying workflows
```

### Documentation

```bash
# Update docs/ directory with new patterns
# Add integration examples
# Maintain template documentation
```

## 📚 Comprehensive Documentation

### 🚀 Getting Started
- **[Getting Started Guide](docs/getting-started.md)** - Complete template setup tutorial with examples
- **[FAQ](docs/faq.md)** - Answers to common questions about AI Track Down's revolutionary framework approach

### 🏗️ Framework Deep Dive  
- **[Architecture Guide](docs/architecture.md)** - Framework design, AI integration patterns, and scaling strategies
- **[Template Reference](docs/template-reference.md)** - Complete template structure reference with examples
- **[Configuration Guide](docs/configuration.md)** - Advanced configuration templates and team setups

### 🔄 Platform Integration
- **[Integration Guide](docs/integrations.md)** - GitHub, Jira, Linear integration patterns with field mapping examples
- **[Best Practices](docs/best-practices.md)** - Proven strategies for AI-human collaboration workflows

### 💡 Examples and Templates
- **[Basic Setup Example](examples/basic-setup/)** - Small team starter template configuration
- **[Enterprise Integration](examples/enterprise-integration/)** - Large organization template setup with multi-team coordination
- **[Sample Tasks](examples/sample-tasks/)** - Real-world epic, issue, and task template examples
- **[Configuration Templates](examples/configs/)** - Ready-to-use config file templates for different scenarios

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

### Quick Contributing Steps

1. Fork the repository
2. Create a feature branch
3. Add or improve templates
4. Update documentation and examples
5. Submit a pull request

## 📋 Roadmap

### Phase 1: Framework Foundation ✅
- [x] Basic template structure and documentation
- [x] Git workflow patterns
- [x] Markdown template formats
- [x] Configuration templates

### Phase 2: AI Integration Templates 🚧
- [x] llms.txt template patterns
- [x] Token tracking templates
- [x] AI context marker templates
- [ ] Advanced AI optimization patterns

### Phase 3: Integration Patterns 📋
- [ ] GitHub Issues integration templates
- [ ] Jira webhook pattern templates
- [ ] Linear API integration patterns
- [ ] Conflict resolution workflow templates

### Phase 4: Tooling Development 📋
- [ ] CLI implementation (separate project)
- [ ] Web UI templates (optional)
- [ ] Analytics template patterns
- [ ] Multi-project template structures

## 📄 License

MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [llms.txt standard](https://llmstxt.org/) for AI context conventions
- [GitHub Flavored Markdown](https://github.github.com/gfm/) for file format
- The open-source community for inspiration and best practices

## 💬 Support

- [GitHub Issues](https://github.com/ai-trackdown/ai-trackdown/issues) - Bug reports and feature requests
- [Discussions](https://github.com/ai-trackdown/ai-trackdown/discussions) - Community discussions and template sharing
- [Documentation](https://ai-trackdown.github.io/ai-trackdown/) - Complete framework documentation

---

**Made with ❤️ for human-AI collaborative development - A documentation framework for the AI era**