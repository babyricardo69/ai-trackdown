# Changelog

All notable changes to ai-trackdown will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Initial project structure and core functionality
- CLI interface for task management
- llms.txt standard support for AI integration
- Token tracking system with cost analysis
- Template system for epics, issues, and tasks
- Git hooks for automatic task status updates
- Configuration system with YAML support

### Changed
- N/A (initial release)

### Deprecated
- N/A (initial release)

### Removed
- N/A (initial release)

### Fixed
- N/A (initial release)

### Security
- N/A (initial release)

## [1.0.0] - 2025-01-07

### Added
- **Core Task Management**
  - Create, read, update, and delete epics, issues, and tasks
  - Markdown-based file storage with YAML frontmatter
  - Hierarchical task organization (epics > issues > tasks)
  - Status tracking and assignment management
  - Label and tagging system

- **AI Integration**
  - Native llms.txt standard implementation
  - Automatic AI context generation
  - Token usage tracking per task/epic/project
  - Cost analysis and budget management
  - AI agent permission system

- **CLI Interface**
  - Complete command-line interface
  - Interactive project initialization
  - List, filter, and search functionality
  - Bulk operations support
  - Rich terminal output with colors and formatting

- **Git Integration**
  - Git hooks for automatic status updates
  - Commit message parsing for task references
  - Branch naming convention support
  - Integration with git workflow

- **File Templates**
  - Epic template with success metrics and token tracking
  - Issue template with acceptance criteria and AI context
  - Task template with implementation details
  - Customizable template system

- **Configuration System**
  - YAML-based configuration
  - Project-specific settings
  - Integration credentials management
  - Token tracking configuration

- **Documentation**
  - Comprehensive README with examples
  - Contributing guidelines
  - API documentation
  - Integration guides

### Technical Details
- Node.js 18+ support
- Zero external dependencies for core functionality
- Git-native storage approach
- Extensible plugin architecture
- Cross-platform compatibility

### Integration Support (Planned)
- GitHub Issues bidirectional sync
- Jira integration with webhooks
- Linear API integration
- Slack/Discord notifications

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

Thank you to all contributors who have helped shape ai-trackdown! 

### Core Team
- Development team members will be listed here as the project grows

### Community Contributors
- Community contributions will be acknowledged here

### Special Thanks
- To the llms.txt standard creators for AI integration guidelines
- To the markdown and git communities for inspiration
- To all early adopters and beta testers