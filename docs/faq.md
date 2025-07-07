# Frequently Asked Questions

This FAQ addresses common questions about ai-trackdown, its revolutionary approach to task management, and practical implementation details.

## 🤔 General Questions

### Q: What makes ai-trackdown different from other task management tools?

**A:** ai-trackdown is the first task management system designed specifically for the AI development era. Unlike traditional tools built for human-only workflows, ai-trackdown treats AI agents as first-class citizens:

- **Native AI Context**: Built-in llms.txt support means AI agents understand your project instantly
- **Token Economics**: Track and optimize AI costs at every level (task/epic/project)
- **Git-Native**: Tasks live where your code lives, not in separate systems
- **Zero Lock-in**: Pure markdown files you can read, edit, and migrate anywhere
- **AI-Optimized**: Structured for minimal token consumption and maximum AI efficiency

Traditional tools require complex APIs and integrations for AI agents to understand project context. ai-trackdown makes your project instantly readable by any AI agent through the llms.txt standard.

### Q: Why markdown files instead of a database?

**A:** Markdown files provide several revolutionary advantages:

1. **Human Readable**: No special tools needed to view or edit tasks
2. **AI Friendly**: Optimal token efficiency for Large Language Models
3. **Git Native**: Perfect version control, branching, and merging
4. **Tool Agnostic**: Works with any editor, IDE, or automation tool
5. **Future Proof**: Will be readable decades from now
6. **Zero Dependencies**: Core functionality works completely offline

A database adds complexity, requires special tools, and creates vendor lock-in. Markdown is the universal interface between humans, AI agents, and development tools.

### Q: How does ai-trackdown handle scale? Can it manage thousands of tasks?

**A:** ai-trackdown is designed for scale through smart architecture:

- **Lazy Loading**: Only loads task files when needed
- **Intelligent Indexing**: Maintains searchable cache for fast queries
- **Git Efficiency**: Leverages git's distributed architecture for performance
- **Incremental Sync**: Only syncs changed items with external systems
- **Smart Context**: Generates targeted AI context, not everything at once

**Performance Targets:**
- 10,000+ tasks per repository
- < 100ms for read operations
- < 500ms for sync operations  
- < 1s for llms.txt generation

The file-based approach actually scales better than databases for development teams because it leverages git's proven distributed architecture.

### Q: Is ai-trackdown suitable for non-technical teams?

**A:** ai-trackdown is designed for development teams but can be adapted for other technical workflows:

**Best Fit:**
- Software development teams using AI assistants
- DevOps teams managing infrastructure with AI
- Data science teams with AI/ML workflows
- Technical writing teams using AI tools

**May Need Adaptation:**
- Pure business teams without technical background
- Teams not using git workflows
- Organizations without AI assistant usage

The markdown-first approach requires comfort with text files and basic git concepts, but provides unmatched flexibility for technical teams.

## 🚀 Getting Started

### Q: How long does it take to set up ai-trackdown?

**A:** Initial setup is designed to be quick:

- **Basic Setup**: 5 minutes (install + init + first task)
- **Team Setup**: 30 minutes (configuration + integrations)
- **Full Integration**: 2-4 hours (external systems + workflows + training)

**Quick Start:**
```bash
npm install -g ai-trackdown
cd your-project
ai-trackdown init
ai-trackdown create epic "Your First Epic"
```

The goal is zero-friction adoption that works with your existing workflows.

### Q: Can I migrate from existing task management tools?

**A:** Yes! ai-trackdown provides migration tools for common platforms:

**Supported Migrations:**
- GitHub Issues → ai-trackdown (full migration)
- Jira → ai-trackdown (issues, epics, custom fields)
- Linear → ai-trackdown (issues, projects, cycles)
- Trello → ai-trackdown (cards, lists, boards)
- CSV/Excel → ai-trackdown (custom mapping)

**Migration Process:**
```bash
# Example: GitHub Issues migration
ai-trackdown migrate github --repo=org/repo --include-closed=false

# Example: Jira migration  
ai-trackdown migrate jira --project=PROJ --map-sprints-to-epics

# Preview before migration
ai-trackdown migrate preview jira --project=PROJ --output=migration-plan.md
```

**What Gets Migrated:**
- Tasks/issues with full history
- Labels, assignees, status
- Comments and attachments (where supported)
- Relationships (blocks, related, etc.)
- Custom fields (with mapping)

### Q: How do I convince my team to switch?

**A:** Focus on the immediate benefits rather than a complete switch:

**Phase 1: Parallel Adoption**
- Start with one epic or project
- Keep existing tools running
- Show token cost tracking benefits
- Demonstrate AI efficiency gains

**Phase 2: Gradual Migration**
- Migrate high-AI-usage projects first
- Use bidirectional sync to maintain existing workflows
- Train team on markdown and git benefits
- Show productivity improvements

**Key Selling Points:**
- "Track AI costs and optimize spending"
- "Never lose context between AI sessions"
- "Your tasks live with your code"
- "Works with any tool you already use"

## 🤖 AI Integration

### Q: Which AI models and providers does ai-trackdown support?

**A:** ai-trackdown is designed to work with any AI provider:

**Tested Providers:**
- **Anthropic**: Claude 3.5 Sonnet, Claude 3 Haiku
- **OpenAI**: GPT-4 Turbo, GPT-3.5 Turbo
- **GitHub**: Copilot Chat
- **Google**: Gemini Pro, Gemini Ultra
- **Cohere**: Command R+
- **Local Models**: Ollama, llama.cpp

**Token Tracking Support:**
```yaml
# Configure any provider
token_providers:
  your_custom_ai:
    model: "custom-model-v1"
    cost_per_1k_input: 0.002
    cost_per_1k_output: 0.008
    api_endpoint: "https://api.yourprovider.com"
```

The llms.txt standard works with any text-capable AI model, making ai-trackdown provider-agnostic.

### Q: How does token tracking work?

**A:** Token tracking provides visibility into AI costs across your project:

**Automatic Tracking:**
- Git commit attribution (when AI agents commit with proper tags)
- API integration tracking (for supported providers)
- Manual reporting by AI agents

**Manual Tracking:**
```bash
# Track token usage for specific work
ai-trackdown tokens add ISSUE-001 --agent=claude --count=1247 --purpose="code review"

# Set budget alerts
ai-trackdown tokens budget --epic=EPIC-001 --limit=50000 --alert-threshold=0.8
```

**Reporting:**
```bash
# Weekly cost analysis
ai-trackdown tokens report --period=week --by=epic

Token Usage Report (This Week)
============================
EPIC-001 (Auth): 12,847 tokens ($0.39) - 25.7% of budget
├── Claude: 8,456 tokens ($0.25) - Requirements & review
├── GPT-4: 3,234 tokens ($0.10) - Code generation  
└── Copilot: 1,157 tokens ($0.04) - Autocomplete

Recommendations:
• EPIC-001 trending over budget - optimize context
• Claude most cost-effective for architecture work
• Consider GPT-3.5 for simple code tasks
```

### Q: What is llms.txt and why should I care?

**A:** llms.txt is an emerging standard for making projects instantly understandable to AI agents:

**The Problem:**
- AI agents lose context between sessions
- Explaining project structure wastes tokens
- AI assistants give generic advice without project context

**The Solution:**
```txt
# /llms.txt - AI agents read this first
## Project Overview
Node.js authentication service with React frontend
Currently implementing OAuth2 social login

## Key Files  
/src/auth/ - Authentication logic
/src/components/auth/ - Login UI components
/tasks/epics/EPIC-001-authentication.md - Current work

## Current Focus
OAuth2 integration (70% complete)
Next: Two-factor authentication

## Quick Commands
View open tasks: ai-trackdown list --status=open
Check progress: ai-trackdown status EPIC-001
```

**Benefits:**
- AI agents understand your project immediately
- Reduced token costs (no repeated explanations)
- Better AI suggestions (contextually aware)
- Standard format works with any AI tool

**Learn More:** [llmstxt.org](https://llmstxt.org)

### Q: How do I optimize AI token usage?

**A:** ai-trackdown provides several optimization strategies:

**1. Smart Context Generation**
```bash
# Generate minimal context for simple tasks
ai-trackdown context TASK-001 --minimal

# Generate deep context for complex work
ai-trackdown context EPIC-001 --depth=3 --include-dependencies
```

**2. Budget Management**
```bash
# Set realistic budgets
ai-trackdown tokens budget --epic=EPIC-001 --limit=25000

# Monitor utilization
ai-trackdown tokens usage --epic=EPIC-001 --trend
```

**3. AI Agent Specialization**
```yaml
# Configure agents for specific roles
ai_agents:
  architect:
    max_tokens: 8000
    specialization: [architecture, security]
  implementer:
    max_tokens: 4000  
    specialization: [coding, testing]
```

**4. Context Optimization**
```markdown
<!-- Efficient AI context -->
## AI Context
<!-- AI_CONTEXT_START -->
OAuth2 implementation for user authentication.
Files: src/auth/oauth.js, src/components/OAuthButton.tsx  
Dependencies: passport-oauth2, express-session
Current issue: PKCE flow implementation
<!-- AI_CONTEXT_END -->
```

## 🔄 Integrations and Sync

### Q: How does bidirectional sync work with GitHub/Jira/Linear?

**A:** Bidirectional sync keeps ai-trackdown and external systems in harmony:

**Sync Strategy:**
1. **Local First**: Your markdown files are the source of truth
2. **Incremental Updates**: Only sync changes since last sync
3. **Conflict Resolution**: Last-write-wins with conflict preservation
4. **Field Mapping**: Flexible mapping between systems

**Example Workflow:**
```bash
# Initial setup
ai-trackdown sync setup github --repo=org/repo

# Regular sync (automatic or manual)
ai-trackdown sync github --incremental

# Conflict resolution when needed
ai-trackdown sync resolve github --interactive
```

**What Syncs:**
- Task titles, descriptions, status
- Labels, assignees, due dates
- Comments and updates
- Relationships (blocks, related)
- Custom fields (with mapping)

**What Doesn't Sync:**
- AI context blocks (kept local)
- Token usage data (ai-trackdown specific)  
- Internal markdown formatting
- Git-specific metadata

### Q: What happens when there are sync conflicts?

**A:** ai-trackdown handles conflicts gracefully:

**Conflict Resolution Strategy:**
```yaml
# Default: Last-write-wins
conflict_resolution:
  strategy: "last_write_wins"
  create_conflict_files: true
  notify_on_conflict: true
```

**When Conflicts Occur:**
1. **Detection**: ai-trackdown identifies conflicting changes
2. **Preservation**: Creates `.conflict` files with both versions
3. **Resolution**: Applies configured strategy (usually last-write-wins)
4. **Notification**: Alerts team to review conflicts

**Manual Resolution:**
```bash
# View conflicts
ai-trackdown sync conflicts --platform=github

# Interactive resolution
ai-trackdown sync resolve --interactive

# Review conflict files
ls tasks/issues/*.conflict
```

**Best Practices:**
- Sync frequently to avoid drift
- Establish clear ownership (who edits where)
- Use automation to minimize manual conflicts

### Q: Can I sync with multiple platforms simultaneously?

**A:** Yes! ai-trackdown supports multi-platform synchronization:

**Configuration:**
```yaml
# .ai-trackdown/config.yaml
sync:
  github:
    enabled: true
    repo: "org/repo"
  jira:
    enabled: true  
    project: "PROJ"
  linear:
    enabled: true
    team: "engineering"
```

**Orchestrated Sync:**
```bash
# Sync all platforms
ai-trackdown sync --all

# Sync in priority order  
ai-trackdown sync --platforms=github,jira,linear --sequential

# Parallel sync (faster, may cause conflicts)
ai-trackdown sync --platforms=github,jira --parallel
```

**Conflict Priorities:**
```yaml
# Resolve multi-platform conflicts by priority
platform_priority:
  - local      # ai-trackdown always wins
  - github     # Primary development platform
  - jira       # Project management
  - linear     # Secondary tracking
```

## 📁 File Structure and Organization

### Q: How should I organize my task files?

**A:** ai-trackdown provides a flexible but recommended structure:

**Standard Structure:**
```
project-root/
├── .ai-trackdown/           # Configuration and metadata
├── tasks/
│   ├── epics/             # High-level project goals
│   ├── issues/            # Development issues/features  
│   └── tasks/             # Granular implementation tasks
├── docs/
│   └── llms-full.txt      # Complete AI context
└── TASKTRACK.md           # Project overview
```

**Scaling Options:**
```
# For large projects
tasks/
├── 2025/                  # Year-based organization
│   ├── q1/               # Quarter organization
│   │   ├── epics/
│   │   ├── issues/
│   │   └── tasks/
├── archived/              # Completed work
└── templates/             # Custom templates
```

**Team-Based Organization:**
```
tasks/
├── teams/
│   ├── frontend/
│   ├── backend/
│   └── devops/
├── shared/                # Cross-team work
└── architecture/          # System-wide decisions
```

### Q: What's the difference between epics, issues, and tasks?

**A:** ai-trackdown uses a three-level hierarchy for organizing work:

**Epics (High-Level Goals)**
- Business objectives or major features
- Multiple weeks/months of work
- Token budgets and success metrics
- Cross-team coordination
- Examples: "User Authentication System", "Payment Integration"

**Issues (Development Features)**  
- Specific features or requirements within an epic
- Days to weeks of work
- Detailed acceptance criteria
- Single team ownership
- Examples: "OAuth2 Login Flow", "Password Reset System"

**Tasks (Implementation Steps)**
- Granular implementation work within an issue
- Hours to days of work
- Specific technical requirements
- Individual developer ownership
- Examples: "Create OAuth Button Component", "Implement JWT Validation"

**Hierarchy:**
```
EPIC-001: User Authentication System
├── ISSUE-001: OAuth2 Login Flow
│   ├── TASK-001: OAuth Button Component
│   ├── TASK-002: OAuth Callback Handler
│   └── TASK-003: User Profile Creation
├── ISSUE-002: Password Reset System
│   ├── TASK-004: Reset Email Template
│   └── TASK-005: Token Validation Logic
└── ISSUE-003: Two-Factor Authentication
    └── TASK-006: TOTP Implementation
```

### Q: Can I customize the file format or templates?

**A:** Yes! ai-trackdown supports extensive customization:

**Custom Templates:**
```bash
# Create custom templates
ai-trackdown template create issue security-review
ai-trackdown template create task frontend-component

# Use custom templates
ai-trackdown create issue "Security audit" --template=security-review
```

**Template Structure:**
```markdown
<!-- templates/issues/security-review.md -->
---
id: {{id}}
type: issue
title: {{title}}
status: todo
labels: [security, review, {{priority}}]
estimate: {{estimate}}
security_level: {{security_level}}
---

# {{title}}

## Security Scope
<!-- Describe security review scope -->

## Acceptance Criteria
- [ ] Threat model analysis
- [ ] Code security review
- [ ] Dependency vulnerability scan
- [ ] Penetration testing
- [ ] Security documentation

## Security Checklist
- [ ] Input validation
- [ ] Authentication/authorization
- [ ] Data encryption
- [ ] Error handling
- [ ] Logging and monitoring

## AI Context
<!-- AI_CONTEXT_START -->
Security review for {{title}}.
Focus areas: {{security_areas}}
Compliance: {{compliance_requirements}}
<!-- AI_CONTEXT_END -->
```

**Custom Fields:**
```yaml
# .ai-trackdown/config.yaml
custom_fields:
  security_level:
    type: select
    options: [low, medium, high, critical]
    required: true
    
  compliance_tags:
    type: array
    options: [gdpr, hipaa, sox, pci]
    
  review_board:
    type: string
    pattern: "^@[a-zA-Z0-9_-]+$"
```

## 🛠️ Development and Customization

### Q: Can I extend ai-trackdown with plugins?

**A:** ai-trackdown supports a plugin architecture for extensions:

**Plugin Types:**
- **Sync Providers**: Add new integration platforms
- **AI Providers**: Support additional AI models
- **Generators**: Custom report and export formats
- **Hooks**: Custom workflow automation
- **Commands**: New CLI commands

**Example Plugin:**
```javascript
// plugins/slack-notifications.js
class SlackNotificationPlugin {
  async onTaskStatusChange(task, oldStatus, newStatus) {
    if (newStatus === 'done') {
      await this.notifySlack(`✅ ${task.title} completed!`);
    }
  }
  
  async onTokenBudgetAlert(epic, usage) {
    await this.notifySlack(`⚠️ Token budget alert: ${epic.title} at ${usage.percent}%`);
  }
}
```

**Plugin Installation:**
```bash
# Install community plugins
ai-trackdown plugin install slack-notifications

# Develop custom plugins
ai-trackdown plugin create my-custom-plugin --template=notification
```

### Q: How do I contribute to ai-trackdown development?

**A:** We welcome contributions! See our [Contributing Guide](../CONTRIBUTING.md) for details:

**Quick Start:**
```bash
# Fork and clone the repository
git clone https://github.com/your-username/ai-trackdown.git
cd ai-trackdown
npm install
npm test
```

**Contribution Areas:**
- **Core Features**: CLI commands, file parsing, sync engines
- **Integrations**: New platform connectors  
- **AI Features**: Token tracking, context optimization
- **Documentation**: Guides, examples, best practices
- **Testing**: Unit tests, integration tests, E2E tests

**Development Process:**
1. Open issue or discussion for new features
2. Fork repository and create feature branch
3. Implement changes with tests
4. Submit pull request with detailed description

### Q: What's the ai-trackdown roadmap?

**A:** Our development roadmap focuses on AI-native capabilities:

**Phase 1: Core System ✅**
- [x] Markdown-based task management
- [x] CLI interface and git integration
- [x] Basic token tracking
- [x] llms.txt generation

**Phase 2: AI Integration 🚧**
- [x] Advanced token tracking and budgeting
- [x] AI context optimization
- [ ] Multi-model AI agent support
- [ ] Intelligent task suggestions

**Phase 3: Sync Capabilities 📋**
- [ ] GitHub Issues bidirectional sync
- [ ] Jira integration with webhooks
- [ ] Linear API integration
- [ ] Multi-platform conflict resolution

**Phase 4: Advanced Features 📋**
- [ ] Real-time collaboration
- [ ] AI-powered project analytics
- [ ] Advanced automation rules
- [ ] Enterprise security features

**Community Requests:**
- Web UI for non-technical stakeholders
- Mobile app for task updates
- Advanced reporting and dashboards
- Integration with more platforms

## 🔧 Troubleshooting

### Q: ai-trackdown commands are not working. What should I check?

**A:** Follow this troubleshooting checklist:

**1. Installation Issues:**
```bash
# Check installation
which ai-trackdown
ai-trackdown --version

# Reinstall if needed
npm uninstall -g ai-trackdown
npm install -g ai-trackdown
```

**2. Project Setup:**
```bash
# Verify project initialization
ls .ai-trackdown/
cat .ai-trackdown/config.yaml

# Re-initialize if needed
ai-trackdown init --force
```

**3. File Permissions:**
```bash
# Check file permissions
ls -la .ai-trackdown/
ls -la tasks/

# Fix permissions if needed
chmod -R 755 .ai-trackdown/
chmod -R 644 tasks/
```

**4. Git Integration:**
```bash
# Check git status
git status
git log --oneline -5

# Verify git hooks
ls -la .git/hooks/
ai-trackdown hooks status
```

### Q: Sync is failing with external systems. How do I debug?

**A:** Debug sync issues systematically:

**1. Test Authentication:**
```bash
# Test credentials
ai-trackdown sync test github --auth-only
ai-trackdown sync test jira --verbose

# Refresh credentials
ai-trackdown sync refresh-auth github
```

**2. Check Sync Status:**
```bash
# Detailed sync status
ai-trackdown sync status --verbose --all-platforms

# View sync history
ai-trackdown sync history github --limit=10 --errors-only
```

**3. Debug Mode:**
```bash
# Enable debug logging
export AITASKTRACK_DEBUG=true
ai-trackdown sync github --verbose 2>&1 | tee sync-debug.log
```

**4. Common Issues:**
- **Rate Limiting**: Wait and retry, or configure rate limits
- **Permission Errors**: Verify API token scopes
- **Network Issues**: Check connectivity and firewall settings
- **Field Mapping**: Review and update field mappings

### Q: Token tracking seems inaccurate. How do I fix it?

**A:** Ensure accurate token tracking:

**1. Verify Configuration:**
```bash
# Check token provider configuration
ai-trackdown config get ai.token_tracking
ai-trackdown tokens providers --list
```

**2. Audit Token Data:**
```bash
# Review token usage
ai-trackdown tokens audit --detailed --period=month

# Check for missing attribution
ai-trackdown tokens unattributed --fix-suggestions
```

**3. Calibrate Costs:**
```bash
# Update provider pricing
ai-trackdown config set ai.providers.claude.cost_per_1k_input=0.003

# Recalculate costs
ai-trackdown tokens recalculate --all --period=month
```

**4. Manual Corrections:**
```bash
# Correct token usage
ai-trackdown tokens correct ISSUE-001 --agent=claude --count=1500 --reason="API tracking failed"

# Bulk corrections
ai-trackdown tokens import corrections.csv
```

## 📚 Learning and Resources

### Q: Where can I learn more about ai-trackdown?

**A:** Comprehensive learning resources:

**Documentation:**
- [Getting Started Guide](getting-started.md) - Quick start tutorial
- [Architecture Guide](architecture.md) - Deep dive into system design
- [Best Practices](best-practices.md) - Proven strategies and patterns
- [Integration Guide](integrations.md) - External platform setup

**Examples:**
- `/examples` folder - Real-world project examples
- `/templates` folder - Starter templates for different project types

**Community:**
- **GitHub Discussions** - Questions, ideas, best practices
- **GitHub Issues** - Bug reports and feature requests
- **Documentation Site** - Searchable guides and references

### Q: How do I stay updated with ai-trackdown developments?

**A:** Stay connected with the community:

- **GitHub Releases** - Subscribe to release notifications
- **Changelog** - Review [CHANGELOG.md](../CHANGELOG.md) for updates
- **Discussions** - Follow GitHub Discussions for announcements
- **Social Media** - Follow project updates and community highlights

**Update Process:**
```bash
# Check for updates
ai-trackdown update --check

# Update to latest version
npm update -g ai-trackdown

# Review breaking changes
ai-trackdown migrate --check --from-version=1.0.0
```

### Q: Can I get commercial support for ai-trackdown?

**A:** ai-trackdown is open source with community support:

**Community Support:**
- GitHub Issues for bug reports
- GitHub Discussions for questions
- Documentation and examples
- Community-contributed plugins

**Professional Services:**
While ai-trackdown itself is free and open source, professional services may be available through community partners for:
- Enterprise setup and training
- Custom integrations and plugins
- Workflow consulting and optimization
- Priority support contracts

**Enterprise Features:**
Future enterprise editions may include:
- Advanced security and compliance
- Dedicated support channels
- Custom professional services
- Enterprise-grade integrations

---

## 💡 Have More Questions?

**Can't find your answer here?**

1. **Search Documentation** - Check other guides in `/docs`
2. **GitHub Discussions** - Ask the community
3. **GitHub Issues** - Report bugs or request features
4. **Examples** - Browse real-world usage patterns

**Contributing to FAQ:**
Found a common question not covered here? Please submit a pull request or open an issue to help improve this FAQ for the community!