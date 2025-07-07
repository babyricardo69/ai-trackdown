# AI Agent Prompt for AI Track Down Framework

## System Prompt for AI Agents

```
You are working with the AI Track Down framework - a documentation-based task management system optimized for AI-human collaboration. This framework uses structured markdown files with YAML frontmatter for task tracking.

## Framework Structure
- **Epics**: High-level initiatives with business goals
- **Issues**: Specific problems or features within epics  
- **Tasks**: Implementation steps within issues
- **AI-TRACKDOWN.md**: Project dashboard and status overview

## Key Files and Templates
- Templates in `/templates/` directory for creating new items
- Examples in `/examples/` directory for reference patterns
- Main project dashboard: `AI-TRACKDOWN.md`
- AI context file: `llms.txt`

## When Working with Tasks

### Creating New Items
1. Copy appropriate template from `/templates/` directory
2. Fill in YAML frontmatter with: id, title, status, assignee, dates
3. Write clear descriptions with acceptance criteria
4. Add AI context markers: `<!-- AI_CONTEXT_START -->` and `<!-- AI_CONTEXT_END -->`
5. Track token usage in frontmatter under `token_usage` section

### Updating Existing Items
1. Update status in YAML frontmatter (pending/in-progress/completed/blocked)
2. Add new findings to description body
3. Update token usage when AI work is performed
4. Maintain relationships (epic/issue/task hierarchy)

### Token Tracking
Always update token usage when you perform work:
```yaml
token_usage:
  total: [cumulative tokens]
  by_agent:
    [agent_name]: [tokens_used]
```

### AI Context Optimization
- Add context markers around technical details AI agents need
- Reference related files, dependencies, and implementation notes
- Include links to related tasks using task IDs

## File Organization
- Use clear, descriptive task IDs (EPIC-001, ISSUE-001, TASK-001)
- Store files in appropriate directories: `/tasks/epics/`, `/tasks/issues/`, `/tasks/tasks/`
- Update `AI-TRACKDOWN.md` dashboard when completing major work
- Generate/update `llms.txt` for project context

## Best Practices
- Be specific in task descriptions and acceptance criteria
- Track all AI token usage for cost analysis
- Use AI context markers for technical implementation details
- Maintain clear relationships between epics, issues, and tasks
- Update project dashboard regularly to reflect current state

This framework is documentation-only - use manual file editing and template copying rather than CLI commands.
```

## Essential Documentation Reading for AI Agents

### Required Reading Order
Before working with any project using AI Track Down, read these files in order:

1. **AI-TRACKDOWN.md** (Project Dashboard)
   - Current project status and active work
   - Sprint goals and completion metrics
   - Token usage and budget status
   - Key achievements and blockers

2. **README.md** (Framework Overview)
   - Project context and revolutionary positioning
   - Key features and value proposition
   - Getting started workflows and examples
   - Integration capabilities and roadmap

3. **llms.txt** (Project Index)
   - Quick project overview and structure
   - Active work items and priorities
   - Quick commands and workflows
   - Documentation file locations

4. **docs/llms-full.txt** (if available)
   - Complete project context and history
   - Detailed task relationships and dependencies
   - Full technical context and implementation notes

### Framework Documentation Structure

#### Core Documentation (/docs/)
- **getting-started.md** - Complete setup and onboarding guide
- **architecture.md** - System design and technical architecture
- **best-practices.md** - Proven strategies and team workflows
- **api-reference.md** - Template usage and configuration reference
- **integrations.md** - Third-party platform integration patterns

#### Templates (/templates/)
- **AI-TRACKDOWN-template.md** - Project dashboard template
- **epic-template.md** - High-level initiative template
- **issue-template.md** - Specific problem/feature template
- **task-template.md** - Implementation step template
- **llms-txt-examples.md** - AI context file examples

#### Examples (/examples/)
- **basic-setup/** - Small team starter configuration
- **enterprise-integration/** - Large organization examples
- **complete-project/** - Full realistic project demonstration
- **sample-project/** - Simple project walkthrough

### Best Practices Reading Protocol

#### Before Starting Any Work
1. **Read Project Context**: Start with AI-TRACKDOWN.md to understand current state
2. **Review Active Tasks**: Check /tasks/ directory for ongoing work and dependencies
3. **Understand Framework**: Read relevant /docs/ files for methodology
4. **Check Examples**: Review /examples/ for similar project patterns
5. **Follow Templates**: Use appropriate templates from /templates/ directory

#### During Work Sessions
1. **Update Token Usage**: Track all AI work in task frontmatter
2. **Follow AI Context Patterns**: Use established markers and documentation
3. **Maintain Relationships**: Keep epic/issue/task hierarchy intact
4. **Reference Documentation**: Link to relevant docs in task descriptions

#### When Creating New Tasks
1. **Copy Correct Template**: Use /templates/ directory for consistency
2. **Follow Naming Conventions**: Use established ID patterns (EPIC-001, etc.)
3. **Include AI Context**: Add implementation details in context markers
4. **Set Proper Relationships**: Link to parent epics/issues appropriately

### Documentation Best Practices for AI Agents

#### Reading Strategy
- **Scan First**: Quick overview of structure and key sections
- **Deep Dive**: Focus on sections relevant to current work
- **Cross-Reference**: Link related documents and examples
- **Update Context**: Note changes and new insights in task descriptions

#### Context Building
- **Start Broad**: Project overview and goals from AI-TRACKDOWN.md
- **Narrow Focus**: Specific task context from individual files
- **Technical Details**: Implementation notes from AI context markers
- **Relationships**: Dependencies and connections between tasks

#### Workflow Integration
- **Document Reading**: Always read project docs before starting work
- **Template Usage**: Follow established patterns from /templates/
- **Example Reference**: Use /examples/ for real-world patterns
- **Best Practice Application**: Apply /docs/best-practices.md guidelines

This documentation reading protocol ensures AI agents understand project context, follow established patterns, and maintain consistency with team workflows.

## Quick Reference

### Task Creation Workflow
1. `cp templates/[type]-template.md tasks/[type]/[ID]-[title].md`
2. Edit YAML frontmatter with task details
3. Fill in description, acceptance criteria, and context
4. Add to appropriate epic/issue hierarchy
5. Update project dashboard

### Status Updates
1. Update `status` field in YAML frontmatter
2. Add progress notes to task description
3. Update token usage if AI work was performed
4. Update project dashboard if milestone reached

### Token Tracking
Always log AI usage:
- Specify agent name (claude, gpt, etc.)
- Record token count for the work session
- Add brief description of work performed
- Update project totals in dashboard

## Integration with Development Workflow

### Git Integration
- Commit task updates with clear messages referencing task IDs
- Use conventional commit format: `type(TASK-ID): description`
- Update task status based on commit keywords (fix/close → completed)

### AI Context Sharing
- Use `llms.txt` for project-wide context
- Add task-specific context in AI context markers
- Reference implementation files and dependencies
- Include token budget and usage guidelines

This framework optimizes for AI-human collaboration while maintaining human oversight and cost visibility.