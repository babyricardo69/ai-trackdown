# Changelog

All notable changes to AI Track Down will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Future features will be listed here

### Changed
- Future changes will be listed here

## [0.2.0] - 2025-07-08

### Added
- **Internal Pull Request Model**
  - Complete internal PR management system independent of GitHub
  - Agent-to-agent PR creation and review workflows
  - User-controlled PR processes for all code changes
  - PR template with YAML frontmatter and structured review sections
  - Quick PR template for minor changes and bug fixes
  - PR review template with comprehensive checklists and approval tracking

- **PR Workflow Documentation**
  - Complete PR lifecycle management (draft → ready → review → approved → merged)
  - Agent-optimized PR creation patterns for AI assistants
  - Manual PR workflows for user-controlled processes
  - PR-to-task integration with automatic status synchronization
  - Security and compliance review workflows
  - Token usage tracking for AI-assisted PR creation and reviews

- **Enhanced Architecture**
  - PR entity added to data model with complete relationship mapping
  - Review entity for structured PR review tracking
  - prs/ directory structure with active, merged, and review subdirectories
  - PR ID generation patterns following existing framework conventions

- **Framework Integration**
  - Seamless integration with existing task/issue hierarchy
  - PR linking to tasks and issues with bidirectional relationships
  - Status-based file organization matching ai-trackdown patterns
  - Complete compatibility with existing template system

### Enhanced Documentation
- Revolutionary agent-to-agent collaboration through internal PR workflows
- GitHub-independent PR model enabling full local control
- Foundation established for CLI tooling implementation
- Enterprise-ready audit trails and compliance tracking

## [0.1.0] - 2025-01-07

### Added
- **Documentation Framework**
  - Complete markdown-based task management documentation
  - Comprehensive design specification and architecture guide
  - Template library for epics, issues, and tasks with YAML frontmatter
  - Example projects demonstrating real-world usage patterns
  - Getting started guide and best practices documentation

- **AI-Native Design**
  - Native llms.txt standard implementation for AI discoverability
  - Token usage tracking templates and examples
  - AI context markers and optimization guidelines
  - Documentation optimized for minimal token consumption

- **Template System**
  - Epic template with success metrics and token tracking
  - Issue template with acceptance criteria and AI context sections  
  - Task template with implementation details and relationships
  - Configuration templates for different project scales

- **Project Structure**
  - Git-native file organization with intuitive directory structure
  - YAML frontmatter specification for task metadata
  - Example configurations for GitHub, Jira, and Linear integration
  - Manual workflow guides for teams without CLI tools

- **Comprehensive Examples**
  - Complete sample project with realistic tasks and epics
  - Enterprise integration examples and configuration
  - Basic setup examples for small teams
  - Configuration examples for different use cases

### Documentation Highlights
- Revolutionary positioning as AI-native task management framework
- Zero-friction onboarding with clear templates and examples
- Self-dogfooding approach using AI Track Down for its own development
- Complete rebranding from TaskTrack to AI Track Down for consistency

---

## Release Notes Format

### Version Numbering
- **MAJOR.MINOR.PATCH** following Semantic Versioning
- **MAJOR**: Breaking changes to API or file format
- **MINOR**: New features, backward compatible
- **PATCH**: Bug fixes, backward compatible

### Change Categories
- **Added**: New features
- **Changed**: Changes in existing functionality
- **Deprecated**: Soon-to-be removed features
- **Removed**: Now removed features
- **Fixed**: Bug fixes
- **Security**: Security vulnerabilities and fixes

### Migration Notes
When breaking changes are introduced, detailed migration instructions will be provided in the release notes.

### Support Policy
- **Current Version**: Full support and active development
- **Previous Major**: Security fixes and critical bug fixes only
- **Older Versions**: Community support only

---

## Contributors

Thank you to all contributors who have helped shape AI Track Down! 

### Core Team
- Development team members will be listed here as the project grows

### Community Contributors
- Community contributions will be acknowledged here

### Special Thanks
- To the llms.txt standard creators for AI integration guidelines
- To the markdown and git communities for inspiration
- To all early adopters and beta testers