# Contributing to AI Track Down

Thank you for your interest in contributing to AI Track Down! This document provides guidelines and information for contributors.

## 🚀 Getting Started

### Prerequisites

- Node.js 18.0.0 or higher
- npm or yarn
- Git
- Basic understanding of markdown and task management concepts

### Development Setup

1. **Fork the repository**
   ```bash
   # Fork on GitHub, then clone your fork
   git clone https://github.com/your-username/ai-trackdown.git
   cd ai-trackdown
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Set up development environment**
   ```bash
   npm run dev
   ```

4. **Run tests to ensure everything works**
   ```bash
   npm test
   ```

## 🎯 How to Contribute

### Reporting Issues

Before creating an issue, please:

1. **Search existing issues** to avoid duplicates
2. **Use the issue templates** provided
3. **Include detailed information**:
   - AI Track Down version
   - Node.js version
   - Operating system
   - Steps to reproduce
   - Expected vs actual behavior
   - Relevant configuration files (anonymized)

### Suggesting Features

We welcome feature suggestions! Please:

1. **Check the roadmap** in README.md first
2. **Open a discussion** before starting implementation
3. **Describe the use case** and business value
4. **Consider AI integration** implications
5. **Think about backward compatibility**

### Code Contributions

#### Branch Naming Convention

- `feature/ISSUE-XXX-description` - New features
- `bugfix/ISSUE-XXX-description` - Bug fixes
- `docs/ISSUE-XXX-description` - Documentation updates
- `refactor/ISSUE-XXX-description` - Code refactoring

#### Pull Request Process

1. **Create a feature branch** from `main`
2. **Make your changes** following our coding standards
3. **Add tests** for new functionality
4. **Update documentation** if needed
5. **Ensure all tests pass**
6. **Submit a pull request** with detailed description

#### Pull Request Requirements

- [ ] Tests pass (`npm test`)
- [ ] Linting passes (`npm run lint`)
- [ ] Documentation updated if needed
- [ ] CHANGELOG.md updated for user-facing changes
- [ ] Commit messages follow conventional commits format

#### Commit Message Format

We use [Conventional Commits](https://www.conventionalcommits.org/):

```
type(scope): description

[optional body]

[optional footer]
```

**Types:**
- `feat` - New feature
- `fix` - Bug fix
- `docs` - Documentation only changes
- `style` - Code style changes (formatting, etc.)
- `refactor` - Code refactoring
- `test` - Adding or modifying tests
- `chore` - Maintenance tasks

**Examples:**
```bash
feat(cli): add token budget command
fix(sync): resolve GitHub API rate limiting
docs(readme): update installation instructions
test(parser): add markdown parsing edge cases
```

## 🏗️ Project Structure

```
AI-Track-Down/
├── bin/                    # CLI executables
│   └── ai-trackdown.js
├── src/                    # Source code
│   ├── commands/           # CLI command implementations
│   ├── core/              # Core functionality
│   ├── sync/              # Integration modules
│   ├── parsers/           # File parsers
│   ├── generators/        # Content generators
│   └── utils/             # Utility functions
├── templates/             # File templates
│   ├── epics/
│   ├── issues/
│   └── tasks/
├── docs/                  # Documentation
├── examples/              # Example configurations
├── tests/                 # Test files
└── scripts/               # Build and utility scripts
```

## 📝 Coding Standards

### JavaScript/Node.js Style

- **ES6+ features** preferred
- **Async/await** over callbacks
- **JSDoc comments** for public APIs
- **Error handling** with descriptive messages
- **Consistent naming** (camelCase for variables, PascalCase for classes)

### File Organization

- **Single responsibility** per module
- **Clear module exports**
- **Logical folder structure**
- **Index files** for folder exports

### Testing Standards

- **Unit tests** for all core functionality
- **Integration tests** for CLI commands
- **Test file naming**: `*.test.js` or `*.spec.js`
- **Describe blocks** for logical grouping
- **Clear test descriptions**

**Example test structure:**
```javascript
describe('TaskParser', () => {
  describe('parseMarkdown()', () => {
    it('should parse valid task markdown with frontmatter', () => {
      // Test implementation
    });
    
    it('should handle malformed frontmatter gracefully', () => {
      // Test implementation
    });
  });
});
```

### Documentation Standards

- **JSDoc** for all public APIs
- **Inline comments** for complex logic
- **README updates** for new features
- **Example usage** in documentation
- **Error message documentation**

## 🤖 AI Integration Guidelines

### llms.txt Standards

When modifying llms.txt generation:

- **Follow the standard** at [llmstxt.org](https://llmstxt.org/)
- **Optimize for token efficiency**
- **Include relevant context only**
- **Test with actual AI models**

### Token Tracking

When working on token tracking features:

- **Accurate attribution** to agents/models
- **Cost calculation** precision
- **Privacy considerations** for usage data
- **Performance impact** minimization

### AI Context Optimization

- **Minimal token usage** for context
- **Relevant information** prioritization
- **Clear context boundaries**
- **Agent-specific formatting**

## 🔍 Testing Guidelines

### Running Tests

```bash
# Run all tests
npm test

# Run tests in watch mode
npm run test:watch

# Run tests with coverage
npm run test:coverage

# Run specific test file
npm test -- tasks.test.js

# Run tests matching pattern
npm test -- --grep "parser"
```

### Test Categories

1. **Unit Tests** - Individual functions/classes
2. **Integration Tests** - Component interactions
3. **CLI Tests** - Command-line interface
4. **Sync Tests** - External integrations
5. **Parser Tests** - File parsing logic

### Test Data

- **Use fixtures** for test data
- **Mock external APIs** appropriately
- **Clean up** test artifacts
- **Isolate tests** from each other

## 📋 Documentation Guidelines

### README Updates

When updating documentation:

- **Keep examples current** with latest features
- **Test all code examples**
- **Update table of contents**
- **Check all links**

### API Documentation

- **Complete parameter descriptions**
- **Return value documentation**
- **Error conditions**
- **Usage examples**
- **Version compatibility**

### Guide Writing

- **Step-by-step instructions**
- **Screenshot guidelines** where helpful
- **Common pitfalls** section
- **Troubleshooting** information

## 🔄 Integration Development

### Adding New Integrations

When adding support for new platforms:

1. **Create integration module** in `src/sync/`
2. **Implement standard interface**
3. **Add configuration options**
4. **Include comprehensive tests**
5. **Update documentation**
6. **Add example configurations**

### Integration Interface

All integrations should implement:

```javascript
class IntegrationBase {
  async authenticate(config) {}
  async sync(options) {}
  async push(items) {}
  async pull(options) {}
  async validateConfig(config) {}
}
```

### Error Handling

- **Graceful degradation** for network issues
- **Clear error messages** for configuration problems
- **Retry logic** for transient failures
- **Rate limiting** respect
- **Comprehensive logging**

## 🚀 Release Process

### Version Management

We follow [Semantic Versioning](https://semver.org/):

- **MAJOR** - Breaking changes
- **MINOR** - New features (backward compatible)
- **PATCH** - Bug fixes (backward compatible)

### Release Checklist

- [ ] All tests pass
- [ ] Documentation updated
- [ ] CHANGELOG.md updated
- [ ] Version bumped in package.json
- [ ] Git tag created
- [ ] npm package published
- [ ] GitHub release created

## 🆘 Getting Help

### Development Support

- **GitHub Discussions** - Design discussions and questions
- **GitHub Issues** - Bug reports and feature requests
- **Code Reviews** - Pull request feedback
- **Documentation** - Comprehensive guides in `/docs`

### Communication Channels

- **GitHub Issues** - Primary communication
- **Pull Request Comments** - Code-specific discussions
- **GitHub Discussions** - General questions and ideas

### Response Times

- **Issues** - Response within 48 hours
- **Pull Requests** - Review within 72 hours
- **Security Issues** - Response within 24 hours

## 🎖️ Recognition

Contributors are recognized in:

- **CONTRIBUTORS.md** - All contributors listed
- **GitHub contributors** - Automatic recognition
- **Release notes** - Major contributors highlighted
- **Social media** - Community contributions shared

## 📜 Code of Conduct

### Our Standards

- **Respectful** communication
- **Constructive** feedback
- **Inclusive** environment
- **Professional** behavior
- **Collaborative** spirit

### Enforcement

Violations of the code of conduct can be reported to the maintainers. All complaints will be reviewed and investigated promptly and fairly.

## 🏁 Quick Start Checklist

Before your first contribution:

- [ ] Fork and clone the repository
- [ ] Install dependencies (`npm install`)
- [ ] Run tests (`npm test`)
- [ ] Create a feature branch
- [ ] Read relevant documentation
- [ ] Set up your development environment
- [ ] Make a small test change to ensure everything works

## 📚 Additional Resources

- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)
- [JavaScript Style Guide](https://github.com/airbnb/javascript)
- [Testing Best Practices](https://github.com/goldbergyoni/javascript-testing-best-practices)
- [Documentation Guide](https://www.writethedocs.org/guide/)

---

**Thank you for contributing to AI Track Down! Your involvement helps make task management better for both humans and AI agents.**