# Contributing to git-ops

Thank you for your interest in contributing to git-ops! We welcome contributions from the community.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [How to Contribute](#how-to-contribute)
- [Development Workflow](#development-workflow)
- [Coding Standards](#coding-standards)
- [Commit Guidelines](#commit-guidelines)
- [Pull Request Process](#pull-request-process)
- [Reporting Bugs](#reporting-bugs)
- [Suggesting Enhancements](#suggesting-enhancements)

## Code of Conduct

This project and everyone participating in it is governed by our [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code.

## Getting Started

1. Fork the repository
2. Clone your fork: `git clone https://github.com/YOUR_USERNAME/git-ops.git`
3. Create a new branch: `git checkout -b feature/your-feature-name`
4. Make your changes
5. Commit your changes following our [commit guidelines](#commit-guidelines)
6. Push to your fork: `git push origin feature/your-feature-name`
7. Open a pull request

## How to Contribute

### Development Setup

```bash
# Clone the repository
git clone https://github.com/InfinityXOneSystems/git-ops.git
cd git-ops

# Install dependencies (if applicable)
# npm install
# or
# pip install -r requirements.txt

# Run tests (if applicable)
# npm test
# or
# pytest
```

## Development Workflow

1. **Create an Issue**: Before starting work, create or find an issue describing what you want to do
2. **Branch**: Create a feature branch from `main` or `develop`
3. **Code**: Write your code following our coding standards
4. **Test**: Ensure all tests pass and add new tests for new functionality
5. **Document**: Update documentation as needed
6. **Commit**: Make commits with clear, descriptive messages
7. **Pull Request**: Submit a PR with a clear description of your changes

## Coding Standards

- Write clean, readable, and maintainable code
- Follow the existing code style in the project
- Comment your code where necessary
- Write meaningful variable and function names
- Keep functions small and focused on a single task

### Language-Specific Guidelines

#### Python
- Follow [PEP 8](https://www.python.org/dev/peps/pep-0008/)
- Use type hints where appropriate
- Write docstrings for functions and classes

#### JavaScript/TypeScript
- Use ES6+ features
- Follow [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
- Use meaningful variable names
- Prefer `const` over `let`, avoid `var`

## Commit Guidelines

We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types
- **feat**: A new feature
- **fix**: A bug fix
- **docs**: Documentation only changes
- **style**: Changes that do not affect the meaning of the code
- **refactor**: A code change that neither fixes a bug nor adds a feature
- **perf**: A code change that improves performance
- **test**: Adding missing tests or correcting existing tests
- **chore**: Changes to the build process or auxiliary tools

### Examples
```
feat(api): add new endpoint for user authentication

fix(validation): resolve edge case in email validation

docs(readme): update installation instructions
```

## Pull Request Process

1. **Update Documentation**: Ensure any new features are documented
2. **Add Tests**: Include tests for new functionality
3. **Update CHANGELOG**: Add an entry describing your changes
4. **Self Review**: Review your own code before submitting
5. **Description**: Provide a clear description of what your PR does
6. **Link Issues**: Reference any related issues using `Fixes #123` or `Closes #456`
7. **Request Review**: Request review from maintainers
8. **Address Feedback**: Respond to and address review comments
9. **Squash Commits**: Consider squashing commits before merging (if requested)

### PR Checklist

- [ ] Code follows the project's coding standards
- [ ] Self-review of code completed
- [ ] Comments added to hard-to-understand areas
- [ ] Documentation updated
- [ ] No new warnings generated
- [ ] Tests added and passing
- [ ] All existing tests passing
- [ ] CHANGELOG.md updated

## Reporting Bugs

When reporting bugs, please use our [bug report template](.github/ISSUE_TEMPLATE/bug_report.md) and include:

- A clear and descriptive title
- Steps to reproduce the issue
- Expected behavior
- Actual behavior
- Screenshots (if applicable)
- Environment details (OS, version, etc.)

## Suggesting Enhancements

When suggesting enhancements, please use our [feature request template](.github/ISSUE_TEMPLATE/feature_request.md) and include:

- A clear and descriptive title
- Detailed description of the proposed feature
- Why this feature would be useful
- Possible implementation approach

## Questions?

Feel free to open a [question issue](.github/ISSUE_TEMPLATE/question.md) or start a [discussion](https://github.com/InfinityXOneSystems/git-ops/discussions).

## License

By contributing, you agree that your contributions will be licensed under the same license as the project.

## Recognition

Contributors will be recognized in our README.md file and release notes.

Thank you for contributing! 🎉
