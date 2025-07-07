# llms.txt Documentation Templates

This document provides templates and examples for creating AI-discoverable project indexes using the llms.txt standard in ai-trackdown projects.

---

## Basic llms.txt Template

### Location: `/.ai-trackdown/llms.txt`

```txt
# [Project Name] - AI Task Index
# Generated: YYYY-MM-DDTHH:mm:ssZ
# Format: llms.txt v1.0
# ai-trackdown Project

## Project Overview
[Brief project description]
Active epics: [count]
Open issues: [count]
Active tasks: [count]
Completed items: [count]

## Current Focus
- [Primary epic or milestone]: [progress %]
- [Key issue]: [status]
- [Important task]: [status]

## Key Directories
/tasks/epics/: High-level project initiatives and features
/tasks/issues/: Development issues and user stories
/tasks/tasks/: Granular implementation tasks
/docs/: Project documentation and AI context
/AI-TRACKDOWN.md: Main project dashboard

## Recent Activity
- [Recent completion or milestone]
- [Current blockers]
- [Upcoming deliverables]

## Team Information
Project Lead: @[username]
Team Size: [number] developers
Current Sprint: [sprint identifier]

## AI Integration
Token Budget: [amount] tokens/month
Current Usage: [amount] tokens ([percentage]% of budget)
Primary AI Agents: [list of agents in use]

## Quick Navigation
View all open work: /tasks/
Project status: /AI-TRACKDOWN.md
Full AI context: /docs/llms-full.txt

## External Systems
GitHub: [sync status] (Last: YYYY-MM-DD)
Jira: [sync status] (Last: YYYY-MM-DD) 
Linear: [sync status] (Last: YYYY-MM-DD)
```

---

## Complete Context Template

### Location: `/docs/llms-full.txt`

```txt
# [Project Name] - Complete AI Context
# Generated: YYYY-MM-DDTHH:mm:ssZ
# Format: ai-trackdown Full Context v1.0

## Executive Summary
[Comprehensive project overview - what, why, goals]

## Project Architecture
[High-level technical architecture]

### Key Technologies
- Technology 1: [Purpose]
- Technology 2: [Purpose]
- Technology 3: [Purpose]

### Core Components
- Component 1: [Description and location]
- Component 2: [Description and location]
- Component 3: [Description and location]

## Active Development

### Current Sprint: [Sprint ID]
Goal: [Sprint objective]
Duration: YYYY-MM-DD to YYYY-MM-DD

### Epic Breakdown

#### EPIC-001: [Epic Title] ([Progress]% complete)
Objective: [What this epic achieves]
Token Budget: [amount] ([percentage]% used)

**Issues:**
- ISSUE-001: [Title] (Status: [status])
  - Assignee: @[username]
  - Priority: [level]
  - Acceptance Criteria: [brief summary]
  - Blockers: [any blockers]
  
- ISSUE-002: [Title] (Status: [status])
  - [Similar details]

**Tasks in Progress:**
- TASK-001: [Title] (@[assignee]) - [status]
  - Files: [key files being modified]
  - Next Steps: [what's next]
  
- TASK-002: [Title] (@[assignee]) - [status]
  - [Similar details]

#### EPIC-002: [Epic Title] ([Progress]% complete)
[Similar structure for each active epic]

## Technical Context

### Codebase Structure
```
src/
├── components/     # React components
├── pages/         # Page components
├── services/      # API and business logic
├── utils/         # Utility functions
└── types/         # TypeScript definitions
```

### Key Files by Feature
**Authentication:**
- src/auth/AuthProvider.tsx: Main auth context
- src/auth/LoginForm.tsx: Login UI component
- src/services/authService.ts: Authentication API

**Payment Processing:**
- src/payment/PaymentForm.tsx: Payment UI
- src/services/paymentService.ts: Stripe integration
- src/webhooks/payment.ts: Payment webhooks

### Database Schema
[Brief overview of key tables/collections]

### API Endpoints
[Key API routes and their purposes]

## Development Guidelines

### Code Standards
- TypeScript for all new code
- ESLint/Prettier for formatting
- Jest for unit tests
- Cypress for e2e tests

### Git Workflow
- Feature branches from main
- Pull request required for merge
- Branch naming: feature/ISSUE-XXX-description

### Testing Requirements
- Unit test coverage: 80% minimum
- Integration tests for API endpoints
- E2E tests for critical user flows

## Recent Technical Decisions

### [YYYY-MM-DD]: [Decision Title]
**Decision:** [What was decided]
**Rationale:** [Why this decision was made]
**Impact:** [How this affects the codebase]
**Alternatives Considered:** [Other options that were evaluated]

### [YYYY-MM-DD]: [Decision Title]
[Similar structure for each decision]

## Known Issues & Technical Debt

### High Priority
- [Issue description]: [Impact and planned resolution]
- [Issue description]: [Impact and planned resolution]

### Medium Priority
- [Issue description]: [Impact and timeline]
- [Issue description]: [Impact and timeline]

## Deployment & Infrastructure

### Environments
- Development: [URL and details]
- Staging: [URL and details]
- Production: [URL and details]

### CI/CD Pipeline
[Brief description of build and deployment process]

### Monitoring & Alerts
[Key metrics and alerting setup]

## AI Agent Guidelines

### Preferred Approach
- Read existing code before making changes
- Follow established patterns and conventions
- Update tests when modifying functionality
- Document decisions in task activity logs

### Token Usage Guidelines
- Log token usage in task files
- Focus on high-impact activities
- Prefer code analysis over code generation
- Validate assumptions before implementation

### Context Optimization
- Reference this file for project context
- Use task-specific AI_CONTEXT sections
- Minimize redundant context requests
- Focus on relevant code sections

## Stakeholder Information

### Key Contacts
- Product Owner: @[username]
- Technical Lead: @[username] 
- DevOps: @[username]
- QA Lead: @[username]

### Communication Channels
- Daily Standups: [time/location]
- Sprint Planning: [schedule]
- Technical Reviews: [schedule]
- Stakeholder Updates: [schedule]

## Project Timeline

### Completed Milestones
- [YYYY-MM-DD]: [Milestone description]
- [YYYY-MM-DD]: [Milestone description]

### Upcoming Milestones
- [YYYY-MM-DD]: [Milestone description]
- [YYYY-MM-DD]: [Milestone description]

### Long-term Roadmap
- Q1 [YYYY]: [High-level goals]
- Q2 [YYYY]: [High-level goals]
- Q3 [YYYY]: [High-level goals]

## Metrics & KPIs

### Development Metrics
- Velocity: [story points per sprint]
- Cycle Time: [average days from start to done]
- Lead Time: [average days from creation to done]
- Bug Rate: [bugs per story point]

### Business Metrics
- [Key business metric]: [current value]
- [Key business metric]: [current value]
- [Key business metric]: [current value]

### AI Efficiency Metrics
- Token utilization: [usage vs budget]
- AI-assisted completion rate: [percentage]
- Context accuracy: [subjective assessment]

---

*This document is automatically updated and should reflect the current state of the project. Last human review: YYYY-MM-DD*
```

---

## Project Summary Templates

### Startup Project Template

```txt
# [Startup Name] - Product Development
# Early-stage product development with lean methodology

## Current Phase
Phase: MVP Development
Target Launch: [date]
Funding Stage: [stage]

## Core Features (MVP)
- [ ] User authentication
- [ ] Core feature 1
- [ ] Core feature 2
- [ ] Payment integration

## Team
Founder: @[name]
CTO: @[name] 
Developers: [count]
Current Runway: [months]

## Key Metrics
Burn Rate: $[amount]/month
Development Velocity: [story points/week]
User Acquisition Goal: [number] beta users

## Technology Choices
Frontend: [technology] - [rationale]
Backend: [technology] - [rationale]
Database: [technology] - [rationale]
Hosting: [provider] - [rationale]

## Immediate Priorities
1. [Priority 1]
2. [Priority 2] 
3. [Priority 3]
```

### Enterprise Project Template

```txt
# [Enterprise Project] - System Integration
# Large-scale enterprise system development

## Project Scope
Business Unit: [unit]
Budget: $[amount]
Timeline: [start] to [end]
Compliance: [requirements]

## Architecture
Pattern: [microservices/monolith/etc]
Integrations: [number] external systems
Users: [number] concurrent users
Uptime SLA: [percentage]

## Team Structure
Project Manager: @[name]
Architect: @[name]
Team Leads: [count]
Developers: [count]
QA: [count]
DevOps: [count]

## Risk Management
High Risks: [count]
Mitigation Plans: [status]
Contingency Budget: [percentage]

## Compliance Requirements
- [Requirement 1]: [status]
- [Requirement 2]: [status]
- [Requirement 3]: [status]

## Integration Points
- [System 1]: [purpose] - [status]
- [System 2]: [purpose] - [status]
- [System 3]: [purpose] - [status]
```

### Open Source Project Template

```txt
# [Project Name] - Open Source Library/Tool
# Community-driven development project

## Project Info
License: [license type]
Language: [primary language]
Contributors: [count]
Stars: [count]
Forks: [count]

## Contribution Guidelines
Contributing: See CONTRIBUTING.md
Code of Conduct: CODE_OF_CONDUCT.md
Issue Templates: .github/ISSUE_TEMPLATE/
PR Process: [description]

## Maintainers
Lead Maintainer: @[username]
Core Team: @[user1], @[user2], @[user3]
Emeritus: @[former maintainers]

## Release Process
Current Version: [version]
Next Release: [planned date]
Release Cycle: [frequency]
Breaking Changes: [policy]

## Community
Discussions: [platform/link]
Chat: [platform/link] 
Meetings: [schedule]
RFC Process: [description]

## Documentation
User Docs: [link]
API Docs: [link]
Contributor Docs: [link]
Examples: [link]
```

---

## AI Context Optimization

### Token-Efficient Patterns

#### Focused Context Blocks
```txt
<!-- AI_CONTEXT_START:AUTHENTICATION -->
This issue focuses on user authentication for the e-commerce platform.
Key files: src/auth/, src/components/auth/
Integrations: OAuth2 (Google, GitHub), JWT tokens
Security: OWASP guidelines, rate limiting, secure sessions
Testing: Unit tests in tests/auth/, e2e in cypress/auth/
Related: EPIC-001 (User Management), ISSUE-003 (Authorization)
<!-- AI_CONTEXT_END:AUTHENTICATION -->
```

#### Hierarchical Context
```txt
<!-- AI_CONTEXT_START:LEVEL_1 -->
Epic-level context: Complete payment processing system
<!-- AI_CONTEXT_END:LEVEL_1 -->

<!-- AI_CONTEXT_START:LEVEL_2 -->
Issue-level context: Stripe webhook implementation
<!-- AI_CONTEXT_END:LEVEL_2 -->

<!-- AI_CONTEXT_START:LEVEL_3 -->
Task-level context: Add webhook endpoint validation
<!-- AI_CONTEXT_END:LEVEL_3 -->
```

#### Code-Specific Context
```txt
<!-- AI_CONTEXT_START:CODE -->
Primary files:
- src/services/paymentService.ts: Main payment logic
- src/webhooks/stripe.ts: Webhook handling
- src/types/payment.ts: Type definitions

Dependencies:
- stripe: Payment processing
- joi: Validation
- winston: Logging

Patterns:
- Async/await for API calls
- Joi schemas for validation
- Winston for structured logging
- Jest for unit testing
<!-- AI_CONTEXT_END:CODE -->
```

### Context Layering Strategy

1. **Global Context** (`/docs/llms-full.txt`)
   - Project overview and architecture
   - Team guidelines and standards
   - Major technical decisions

2. **Epic Context** (`EPIC-XXX.md` files)
   - Feature-area specific information
   - Business requirements and goals
   - Success criteria and metrics

3. **Issue Context** (`ISSUE-XXX.md` files)
   - Specific functionality details
   - Acceptance criteria
   - Technical requirements

4. **Task Context** (`TASK-XXX.md` files)
   - Implementation specifics
   - File locations and patterns
   - Testing requirements

---

## Maintenance and Updates

### Automated Generation

While ai-trackdown is documentation-focused, these examples show what an automated system might generate:

```txt
# Auto-generated sections (examples)
## Recent Commits
- abc1234: feat(auth): Add OAuth2 support (ISSUE-001)
- def5678: fix(payment): Resolve webhook validation (TASK-042)
- ghi9012: docs: Update API documentation (EPIC-002)

## Active Branches
- feature/ISSUE-001-oauth-integration (ahead 3 commits)
- bugfix/TASK-042-webhook-fix (ahead 1 commit)
- epic/EPIC-003-api-redesign (ahead 12 commits)

## Token Usage (Last 7 Days)
Total: 12,847 tokens
By Agent:
- Claude: 8,234 tokens (64%)
- GPT-4: 3,456 tokens (27%)
- Copilot: 1,157 tokens (9%)

By Task Type:
- Requirements analysis: 45%
- Code review: 30%
- Architecture discussion: 15%
- Bug investigation: 10%
```

### Manual Update Guidelines

1. **Daily Updates**
   - Update task statuses
   - Log completed work
   - Update blockers/dependencies

2. **Weekly Updates**
   - Refresh project metrics
   - Update sprint progress
   - Review and update priorities

3. **Sprint Boundaries**
   - Archive completed items
   - Update epic progress
   - Refresh project roadmap

4. **Monthly Reviews**
   - Full context review and cleanup
   - Update technical decisions log
   - Refresh team and contact information

---

These templates provide a foundation for creating AI-discoverable project documentation that supports both human and AI collaboration while maintaining the pure markdown approach of ai-trackdown.
