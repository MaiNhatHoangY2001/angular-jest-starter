# CI/CD Pipeline Documentation

## Overview

This project includes a comprehensive GitHub Actions CI/CD pipeline that ensures code quality, security, and automated deployment.

## Workflows

### 1. CI Pipeline (`ci.yml`)

**Triggered on:** Push to `main`/`develop` branches, Pull requests

**Jobs:**

- **Code Quality Checks**: Runs on Node.js 18.x and 20.x
  - Code formatting validation (Prettier)
  - ESLint code analysis
  - TypeScript compilation check
  - Jest unit tests with coverage
  - Build verification
  - Coverage upload to Codecov
- **Security Audit**: NPM audit and dependency vulnerability checks
- **Lighthouse CI**: Performance and accessibility testing (PR only)

### 2. PR Validation (`pr-validation.yml`)

**Triggered on:** Pull request events

**Features:**

- Merge conflict detection
- Commit message validation (Conventional Commits)
- TODO/FIXME comment detection
- Bundle size impact analysis
- Angular-specific structure validation
- Coverage reporting on PR
- Auto-merge for dependabot PRs

### 3. Release Pipeline (`release.yml`)

**Triggered on:** Tags starting with 'v', push to `main`

**Features:**

- Production build and testing
- GitHub release creation with artifacts
- GitHub Pages deployment for main branch

## Setup Requirements

### 1. Repository Secrets

Add these secrets in your GitHub repository settings:

```
LHCI_GITHUB_APP_TOKEN    # Optional: For Lighthouse CI
CODECOV_TOKEN            # Optional: For Codecov integration
```

### 2. Branch Protection Rules

Configure these rules for your `main` branch:

- Require status checks to pass
- Require branches to be up to date
- Require review from code owners
- Restrict pushes to matching branches

### 3. Codecov Integration (Optional)

1. Visit [codecov.io](https://codecov.io)
2. Connect your GitHub repository
3. Add the `CODECOV_TOKEN` to repository secrets

### 4. Lighthouse CI Setup (Optional)

1. Install Lighthouse CI: `npm install -g @lhci/cli`
2. Configure in `lighthouserc.json`
3. Add `LHCI_GITHUB_APP_TOKEN` for GitHub integration

## Code Quality Standards

### Commit Messages

Follow Conventional Commits format:

```
type(scope): description

feat: add user authentication
fix: resolve memory leak in component
docs: update API documentation
```

### Code Standards

- **ESLint**: Enforces coding standards and best practices
- **Prettier**: Automatic code formatting
- **TypeScript**: Strict type checking enabled
- **Angular**: Standalone components, signals, OnPush strategy
- **Testing**: Minimum 80% code coverage required

### Performance Metrics

- Performance: ≥80%
- Accessibility: ≥90%
- Best Practices: ≥80%
- SEO: ≥80%

## Running Locally

### Code Quality Checks

```bash
# Run all linting checks
npm run lint:check

# Format code
npm run format

# Run tests with coverage
npm run test:ci

# Check formatting
npm run format:check
```

### Security Auditing

```bash
# Check for vulnerabilities
npm audit

# Run audit CI
npx audit-ci --moderate
```

## Troubleshooting

### Common Issues

1. **ESLint Failures**: Check `eslint.config.js` configuration
2. **Test Failures**: Ensure Jest setup in `jest.config.ts` and `setup-jest.ts`
3. **Build Failures**: Verify TypeScript configuration in `tsconfig.json`
4. **Coverage Issues**: Check coverage thresholds in `jest.config.ts`

### Pipeline Debugging

1. Check GitHub Actions logs for detailed error messages
2. Run commands locally to reproduce issues
3. Verify all required files are committed
4. Ensure Node.js version compatibility

## Maintenance

### Regular Updates

- Update GitHub Actions versions quarterly
- Keep dependencies updated via Dependabot
- Review and update code quality thresholds
- Monitor pipeline performance and adjust as needed

### Monitoring

- Watch for failed builds and address promptly
- Monitor coverage trends
- Review security audit reports
- Check bundle size impacts
