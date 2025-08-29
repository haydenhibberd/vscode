# Authentication Testing Guide

This document provides comprehensive guidance for running authentication tests in the VS Code codebase, including environment setup, test execution procedures, and troubleshooting common issues.

## Overview

VS Code includes authentication functionality across multiple components:
- **CLI Authentication**: Device code flow for Microsoft and GitHub accounts
- **GitHub Authentication Extension**: OAuth server and authentication flow
- **Microsoft Authentication Extension**: Scope management and telemetry
- **Core Authentication Service**: Session management and provider registration

## Test Locations

### 1. CLI Authentication Tests
- **Location**: `cli/src/auth.rs`
- **Language**: Rust
- **Test Framework**: Cargo built-in testing
- **Coverage**: Device code flow, credential storage, token management

### 2. GitHub Authentication Tests
- **Location**: `extensions/github-authentication/src/test/node/authServer.test.ts`
- **Language**: TypeScript
- **Test Framework**: Mocha
- **Coverage**: LoopbackAuthServer functionality, OAuth callback handling

### 3. Microsoft Authentication Tests
- **Location**: `extensions/microsoft-authentication/src/common/test/scopeData.test.ts`
- **Language**: TypeScript
- **Test Framework**: Mocha
- **Coverage**: Scope data processing, client ID extraction, tenant handling

### 4. Core Authentication Service Tests
- **Location**: `src/vs/workbench/api/test/browser/extHostAuthentication.integrationTest.ts`
- **Language**: TypeScript
- **Test Framework**: Mocha (Browser)
- **Coverage**: Session creation, provider registration, concurrent operations

## Environment Setup

### Prerequisites

Before running authentication tests, ensure your development environment is properly configured:

#### System Dependencies
```bash
# Install required system packages for VS Code development
sudo apt-get update
sudo apt-get install -y \
    libkrb5-dev \
    libgssapi-krb5-2 \
    pkg-config \
    libx11-dev \
    libxkbfile-dev
```

#### Playwright Browser Dependencies (for browser tests)
```bash
# Install Playwright browser dependencies
sudo apt-get install -y \
    libgtk-4-1 \
    libgraphene-1.0-0 \
    libxslt1.1 \
    libwoff2-1.0.2 \
    libvpx7 \
    libevent-2.1-7 \
    libopus0 \
    libenchant-2-2 \
    libsecret-1-0 \
    libhyphen0 \
    libmanette-0.2-0 \
    libgles2-mesa0
```

#### Node.js and Dependencies
```bash
# Install Node.js dependencies
npm install --ignore-scripts
```

#### Rust Environment (for CLI tests)
```bash
# Rust should be available via rustup
# CLI tests use cargo test
```

### VS Code Build Dependencies

The VS Code build system requires additional dependencies that may not be installed with `--ignore-scripts`:

```bash
# If you encounter missing module errors like 'ternary-stream'
npm install  # Full install without --ignore-scripts flag
```

## Running Tests

### 1. CLI Authentication Tests

```bash
cd cli
cargo test --lib
```

**Expected Output**: All tests should pass with output similar to:
```
running 25 tests
test result: ok. 25 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

### 2. GitHub Authentication Tests

```bash
# Using VS Code test infrastructure
npm run test-extension -- --extensionDevelopmentPath=./extensions/github-authentication

# Alternative: Direct mocha execution (requires compiled output)
cd extensions/github-authentication
npm run compile
npx mocha out/test/**/*.test.js
```

### 3. Microsoft Authentication Tests

```bash
# Using VS Code test infrastructure
npm run test-extension -- --extensionDevelopmentPath=./extensions/microsoft-authentication

# Alternative: Direct mocha execution (requires compiled output)
cd extensions/microsoft-authentication
npm run compile
npx mocha out/test/**/*.test.js
```

### 4. Core Authentication Service Tests

```bash
# Browser-based integration tests
npm run test-browser -- --grep "authentication"

# Alternative: Specific test file
npm run test-browser -- --grep "extHostAuthentication"
```

## Test Details

### CLI Authentication Tests (`cli/src/auth.rs`)

**Test Coverage**:
- Device code flow implementation
- Credential storage and retrieval
- Token expiration handling
- Authentication state management

**Key Test Functions**:
- Token validation and refresh
- Keyring integration
- Error handling for authentication failures

### GitHub Authentication Tests (`authServer.test.ts`)

**Test Cases**:
1. **Server Redirect Test**: Verifies `/signin` endpoint redirects to starting URL
2. **Missing Parameters Test**: Ensures 400 status for missing callback parameters
3. **Valid Callback Test**: Confirms proper handling of valid OAuth callback parameters

**Test Structure**:
```typescript
suite('LoopbackAuthServer', () => {
    test('should redirect to starting URL', async () => {
        // Test implementation
    });
    
    test('should return 400 for missing parameters', async () => {
        // Test implementation
    });
    
    test('should handle valid callback parameters', async () => {
        // Test implementation
    });
});
```

### Microsoft Authentication Tests (`scopeData.test.ts`)

**Test Coverage** (19 test cases):
- Default scope inclusion and deduplication
- Alphabetical scope sorting
- Space-separated scope string generation
- OIDC scope handling with TACK ON scopes
- Internal VS Code scope filtering
- Client ID extraction from scopes
- Tenant ID processing
- Authorization server URL parsing

**Key Test Categories**:
- Scope processing and validation
- Client configuration extraction
- Tenant and authorization server handling

### Core Authentication Service Tests (`extHostAuthentication.integrationTest.ts`)

**Integration Test Coverage**:
- Authentication session creation
- Multiple account handling
- Provider registration and management
- Concurrent authentication operations
- Error scenario handling
- Silent authentication requests

## Troubleshooting

### Common Issues

#### 1. Missing Build Dependencies
**Error**: `Cannot find module 'ternary-stream'`
**Solution**: Run full npm install without `--ignore-scripts` flag

#### 2. Playwright Browser Dependencies
**Error**: `Host system is missing dependencies to run browsers`
**Solution**: Install required system libraries listed in Environment Setup

#### 3. VS Code Extension Tests Not Running
**Error**: `Cannot find module 'vscode'`
**Solution**: Use VS Code test infrastructure instead of direct mocha execution

#### 4. TypeScript Compilation Issues
**Error**: `Unknown file extension ".ts"`
**Solution**: Ensure proper TypeScript configuration and use compiled output

### Environment Validation

Before running tests, validate your environment:

```bash
# Check Node.js version
node --version  # Should be compatible with VS Code requirements

# Check npm dependencies
npm list --depth=0

# Verify Rust installation (for CLI tests)
cargo --version

# Test VS Code compilation
npm run compile
```

### Test Execution Checklist

- [ ] System dependencies installed
- [ ] Node.js dependencies installed
- [ ] VS Code compiles successfully
- [ ] Playwright browsers available (for browser tests)
- [ ] Rust toolchain available (for CLI tests)

## Continuous Integration

When running tests in CI environments:

1. **Install all system dependencies** before test execution
2. **Use headless browser mode** for Playwright tests
3. **Set appropriate timeouts** for authentication flows
4. **Configure test reporters** for CI output format

## Contributing

When adding new authentication tests:

1. Follow existing test patterns and naming conventions
2. Include both positive and negative test cases
3. Mock external authentication services appropriately
4. Update this documentation with new test locations
5. Ensure tests are deterministic and don't require real credentials

## Security Considerations

- **Never commit real credentials** to test files
- **Use mock authentication providers** for testing
- **Validate token handling** in test scenarios
- **Test error conditions** for security edge cases

---

For questions or issues with authentication testing, refer to the VS Code development documentation or create an issue in the repository.
