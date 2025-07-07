# Pull Request

## Overview
<!-- Provide a brief summary of what this PR does -->

## Type of Change
<!-- Check all that apply -->
- [ ] 🐛 Bug fix (non-breaking change which fixes an issue)
- [ ] ✨ New feature (non-breaking change which adds functionality)
- [ ] 💥 Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] 📚 Documentation update
- [ ] 🔧 Code refactoring (no functional changes)
- [ ] ⚡ Performance improvement
- [ ] 🧪 Test coverage improvement
- [ ] 🔗 Integration/sync improvement
- [ ] 🤖 AI/LLM enhancement

## Related Issues
<!-- Link to related issues -->
- Fixes #___
- Related to #___
- Part of #___

## Changes Made
<!-- Describe the changes in detail -->

### Core Changes
- [ ] Modified CLI commands: [list commands]
- [ ] Updated file formats: [describe changes]
- [ ] Changed configuration: [describe config changes]
- [ ] Added new integrations: [list platforms]
- [ ] Enhanced AI features: [describe AI improvements]

### Files Modified
<!-- List key files that were changed -->
- `src/`: [brief description]
- `docs/`: [brief description]
- `tests/`: [brief description]

## AI-Native Features
<!-- If this PR affects AI integration -->
- [ ] Improves token tracking accuracy
- [ ] Enhances AI context generation
- [ ] Optimizes token consumption
- [ ] Adds new AI provider support
- [ ] Updates llms.txt generation
- [ ] Improves AI agent workflows

## Testing Performed
<!-- Describe how you tested these changes -->

### Automated Tests
- [ ] All existing tests pass
- [ ] New tests added for new functionality
- [ ] Integration tests updated
- [ ] E2E tests verify changes

### Manual Testing
- [ ] CLI commands tested manually
- [ ] Integration sync tested
- [ ] Token tracking verified
- [ ] File format changes validated
- [ ] Cross-platform compatibility verified

### Test Cases
<!-- Describe specific test scenarios -->
1. **Test case 1**: [description] ✅/❌
2. **Test case 2**: [description] ✅/❌
3. **Test case 3**: [description] ✅/❌

## Breaking Changes
<!-- If this is a breaking change, describe the impact -->
- [ ] No breaking changes
- [ ] Breaking changes (describe below)

### Migration Guide
<!-- If breaking changes exist, provide migration instructions -->
```bash
# Commands to migrate existing installations
```

## Configuration Changes
<!-- If configuration format changed -->
```yaml
# Example of new/changed configuration
```

## Documentation Updates
- [ ] README.md updated
- [ ] CLI help text updated
- [ ] API documentation updated
- [ ] Integration guides updated
- [ ] Examples updated
- [ ] FAQ updated

## Performance Impact
<!-- Describe any performance implications -->
- [ ] No performance impact
- [ ] Improves performance: [describe how]
- [ ] May impact performance: [describe impact and mitigation]

### Benchmarks
<!-- If applicable, provide before/after performance data -->
| Operation | Before | After | Improvement |
|-----------|--------|-------|-------------|
| [Operation] | [time] | [time] | [%] |

## Deployment Checklist
<!-- For maintainers -->
- [ ] Version number updated
- [ ] CHANGELOG.md updated
- [ ] Migration scripts (if needed)
- [ ] Documentation deployed
- [ ] Integration tests pass

## AI Token Usage
<!-- If this PR was developed with AI assistance -->
**AI Assistance Used:**
- [ ] Code generation: [model] - [tokens] tokens
- [ ] Code review: [model] - [tokens] tokens  
- [ ] Documentation: [model] - [tokens] tokens
- [ ] Testing: [model] - [tokens] tokens

**Total Token Cost:** [amount] tokens (~$[cost])

## Screenshots/Examples
<!-- Add screenshots or examples if helpful -->

### Before
```bash
# Example of old behavior
```

### After  
```bash
# Example of new behavior
```

## Reviewer Notes
<!-- Anything specific for reviewers to focus on -->

### Areas of Focus
- [ ] Security implications
- [ ] Performance impact
- [ ] Breaking change validation
- [ ] Integration compatibility
- [ ] AI feature accuracy

### Review Checklist
- [ ] Code follows project style guidelines
- [ ] Self-review completed
- [ ] Tests cover new functionality
- [ ] Documentation is accurate and complete
- [ ] No sensitive information exposed
- [ ] Token tracking is accurate (if applicable)

## Additional Context
<!-- Add any other context or notes for reviewers -->

---

## Pre-Submission Checklist
<!-- Complete before submitting -->
- [ ] I have read the [Contributing Guidelines](../CONTRIBUTING.md)
- [ ] My code follows the project's coding standards
- [ ] I have performed a self-review of my code
- [ ] I have commented my code, particularly hard-to-understand areas
- [ ] I have made corresponding changes to the documentation
- [ ] My changes generate no new warnings
- [ ] I have added tests that prove my fix is effective or my feature works
- [ ] New and existing unit tests pass locally with my changes
- [ ] Any dependent changes have been merged and published