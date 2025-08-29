# VS Code Testing Documentation

This directory contains comprehensive testing guides and documentation for the VS Code codebase.

## Available Guides

### [Authentication Testing Guide](./AUTHENTICATION_TESTING.md)
Complete guide for running authentication tests across all VS Code components:
- CLI authentication tests (Rust/Cargo)
- GitHub authentication extension tests
- Microsoft authentication extension tests  
- Core authentication service integration tests

Includes environment setup, troubleshooting, and detailed test coverage information.

## Contributing to Testing Documentation

When adding new testing documentation:

1. **Follow the established format** with clear sections for setup, execution, and troubleshooting
2. **Include specific commands** and expected outputs
3. **Document environment requirements** and dependencies
4. **Provide troubleshooting guidance** for common issues
5. **Update this README** to reference new guides

## Testing Best Practices

- **Run tests locally** before submitting changes
- **Document environment setup** requirements clearly
- **Include both positive and negative test cases**
- **Use appropriate test isolation** and mocking
- **Follow security best practices** for credential handling

For general VS Code development and testing information, see the main [CONTRIBUTING.md](../../CONTRIBUTING.md) file.
