# Contributing to AuthFramework JavaScript SDK

We love your input! We want to make contributing to the AuthFramework JavaScript SDK as easy and transparent as possible, whether it's:

- Reporting a bug
- Discussing the current state of the code
- Submitting a fix
- Proposing new features
- Becoming a maintainer

## Development Process

We use GitHub to host code, to track issues and feature requests, as well as accept pull requests.

### Pull Requests

Pull requests are the best way to propose changes to the codebase. We actively welcome your pull requests:

1. Fork the repo and create your branch from `main`.
2. If you've added code that should be tested, add tests.
3. If you've changed APIs, update the documentation.
4. Ensure the test suite passes.
5. Make sure your code lints.
6. Issue that pull request!

## Development Setup

### Prerequisites

- Node.js 16+ (recommend using [nvm](https://github.com/nvm-sh/nvm))
- npm, yarn, or pnpm
- Git

### Setup Development Environment

```bash
# Clone your fork
git clone https://github.com/yourusername/authframework-js.git
cd authframework-js

# Install dependencies
npm install

# Build the library
npm run build

# Run tests
npm test
```

### Scripts

```bash
# Build the library (both ESM and CommonJS)
npm run build

# Build in watch mode
npm run build:watch

# Run tests
npm test

# Run tests in watch mode
npm run test:watch

# Run tests with coverage
npm run test:coverage

# Lint code
npm run lint

# Fix linting issues
npm run lint:fix

# Type check
npm run typecheck

# Clean build directory
npm run clean
```

## Code Style

- We use [ESLint](https://eslint.org/) for code linting
- We use [Prettier](https://prettier.io/) for code formatting
- We follow [TypeScript strict mode](https://www.typescriptlang.org/tsconfig#strict)
- We use [conventional commits](https://www.conventionalcommits.org/)
- All code must be TypeScript with proper type definitions
- Prefer modern JavaScript features (ES2020+)
- Use meaningful variable and function names
- Write JSDoc comments for public APIs

## Testing

- Write tests for all new functionality
- Maintain or improve test coverage
- Use Jest for testing framework
- Mock external dependencies
- Test both success and error cases
- Test in multiple environments (Node.js, browsers)

### Test Structure

```typescript
describe('AuthService', () => {
  describe('login', () => {
    it('should successfully login with valid credentials', async () => {
      // Test implementation
    });

    it('should throw error with invalid credentials', async () => {
      // Test implementation
    });
  });
});
```

## Documentation

- All public APIs must have JSDoc comments
- Update README.md if you change functionality
- Add examples for new features
- Update TypeScript definitions
- Keep documentation in sync with code

### JSDoc Style

```typescript
/**
 * Authenticates a user with username and password.
 * 
 * @param username - The user's username or email
 * @param password - The user's password
 * @param rememberMe - Whether to extend session lifetime
 * @returns Promise resolving to authentication response
 * @throws {AuthFrameworkError} When authentication fails
 * 
 * @example
 * ```typescript
 * const response = await client.auth.login('user', 'pass');
 * console.log(response.accessToken);
 * ```
 */
async login(username: string, password: string, rememberMe?: boolean): Promise<LoginResponse> {
  // Implementation
}
```

## Commit Messages

We follow [Conventional Commits](https://www.conventionalcommits.org/):

- `feat:` for new features
- `fix:` for bug fixes
- `docs:` for documentation changes
- `test:` for test changes
- `refactor:` for code refactoring
- `style:` for formatting changes
- `chore:` for maintenance tasks
- `perf:` for performance improvements
- `ci:` for CI/CD changes

Examples:

```
feat: add support for OAuth2 authentication
fix: handle network timeouts in auth requests
docs: update React integration examples
test: add tests for password reset flow
refactor: simplify token validation logic
```

## Build System

We use [Rollup](https://rollupjs.org/) for building the library:

- **ESM build**: `dist/index.esm.js` - For modern bundlers
- **CommonJS build**: `dist/index.js` - For Node.js and older bundlers
- **Type definitions**: `dist/index.d.ts` - TypeScript definitions
- **Source maps**: Generated for all builds

## Browser Compatibility

- Modern browsers (Chrome 80+, Firefox 72+, Safari 13+, Edge 80+)
- Node.js 16+
- React Native 0.60+
- Electron 10+

## Issue Reporting

We use GitHub issues to track public bugs. Report a bug by [opening a new issue](https://github.com/authframework/authframework-js/issues/new).

**Great Bug Reports** tend to have:

- A quick summary and/or background
- Steps to reproduce
  - Be specific!
  - Give sample code if you can
- What you expected would happen
- What actually happens
- Environment information (Node.js version, browser, OS)
- Notes (possibly including why you think this might be happening)

## Feature Requests

We welcome feature requests! Please:

1. Check if the feature already exists or is planned
2. Open an issue describing the feature
3. Explain the use case and benefit
4. Consider the impact on bundle size
5. Be open to discussion and feedback

## Security Issues

Please do not report security vulnerabilities through public GitHub issues. Instead, send an email to security@authframework.dev.

## Release Process

1. Update version in `package.json`
2. Update `CHANGELOG.md`
3. Create a git tag: `git tag v1.x.x`
4. Push tag: `git push origin v1.x.x`
5. GitHub Actions will automatically build and publish to npm

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

## Questions?

Feel free to open a [discussion](https://github.com/authframework/authframework-js/discussions) if you have questions about contributing!
