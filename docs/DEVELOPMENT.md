# Development Guide

## Setting Up Your Development Environment

### Prerequisites

- Git 2.30+
- Code editor (VS Code recommended)
- Terminal/Command line

### Initial Setup

1. **Fork the repository**
   ```bash
   # Visit https://github.com/InfinityXOneSystems/git-ops
   # Click "Fork" button
   ```

2. **Clone your fork**
   ```bash
   git clone https://github.com/YOUR_USERNAME/git-ops.git
   cd git-ops
   ```

3. **Add upstream remote**
   ```bash
   git remote add upstream https://github.com/InfinityXOneSystems/git-ops.git
   ```

4. **Install dependencies**
   ```bash
   # If Node.js project:
   # npm install
   
   # If Python project:
   # pip install -r requirements.txt
   # pip install -r requirements-dev.txt
   ```

## Development Workflow

### Creating a Feature Branch

```bash
# Update your main branch
git checkout main
git pull upstream main

# Create a new feature branch
git checkout -b feature/your-feature-name
```

### Making Changes

1. Make your changes in small, logical commits
2. Follow the coding standards
3. Add tests for new functionality
4. Update documentation as needed

### Testing Your Changes

```bash
# Run tests
# npm test
# or
# pytest

# Run linting
# npm run lint
# or
# flake8 .

# Check for security issues
# npm audit
# or
# safety check
```

### Committing Changes

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```bash
git add .
git commit -m "feat: add new feature"
git commit -m "fix: resolve bug in validation"
git commit -m "docs: update README"
```

### Pushing Changes

```bash
git push origin feature/your-feature-name
```

### Creating a Pull Request

1. Go to GitHub and navigate to your fork
2. Click "New Pull Request"
3. Fill out the PR template
4. Link any related issues
5. Request review from maintainers

## Code Style Guide

### General Principles

- Write clear, self-documenting code
- Use meaningful variable names
- Keep functions small and focused
- Comment complex logic
- Avoid code duplication

### Naming Conventions

- **Variables**: `camelCase` or `snake_case` (depending on language)
- **Functions**: `camelCase()` or `snake_case()` (depending on language)
- **Classes**: `PascalCase`
- **Constants**: `UPPER_SNAKE_CASE`
- **Files**: `kebab-case.ext` or `snake_case.ext`

### Code Organization

```
src/
├── core/          # Core functionality
├── utils/         # Utility functions
├── config/        # Configuration
├── models/        # Data models
├── services/      # Business logic
└── tests/         # Test files
```

## Testing

### Writing Tests

```javascript
// Example: JavaScript/Jest
describe('Feature Name', () => {
  it('should do something', () => {
    // Arrange
    const input = 'test';
    
    // Act
    const result = someFunction(input);
    
    // Assert
    expect(result).toBe('expected');
  });
});
```

```python
# Example: Python/pytest
def test_feature_name():
    # Arrange
    input_data = 'test'
    
    # Act
    result = some_function(input_data)
    
    # Assert
    assert result == 'expected'
```

### Running Tests

```bash
# Run all tests
npm test
# or
pytest

# Run specific test file
npm test path/to/test.js
# or
pytest path/to/test.py

# Run with coverage
npm test -- --coverage
# or
pytest --cov=src
```

## Debugging

### Common Issues

1. **Merge Conflicts**
   ```bash
   git fetch upstream
   git rebase upstream/main
   # Resolve conflicts
   git rebase --continue
   ```

2. **Reset to Upstream**
   ```bash
   git fetch upstream
   git reset --hard upstream/main
   git push origin main --force
   ```

3. **Undo Last Commit**
   ```bash
   git reset --soft HEAD~1  # Keep changes
   git reset --hard HEAD~1  # Discard changes
   ```

## Tools and Resources

### Recommended VS Code Extensions

- GitLens
- ESLint / Pylint
- Prettier
- GitHub Copilot
- Code Spell Checker

### Useful Commands

```bash
# View git log
git log --oneline --graph --decorate

# Check what changed
git diff

# Stash changes
git stash
git stash pop

# Clean untracked files
git clean -fd
```

## Getting Help

- Check existing documentation
- Search closed issues
- Ask in GitHub Discussions
- Join community chat (if available)

## Additional Resources

- [Git Documentation](https://git-scm.com/doc)
- [GitHub Docs](https://docs.github.com)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Semantic Versioning](https://semver.org/)

---

Happy coding! 🚀
