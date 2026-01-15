# Quick Start Guide

Welcome to git-ops! This guide will help you get started quickly.

## 📦 For Users

### Prerequisites
- Git installed on your system
- GitHub account (for contributing)

### Installation

```bash
# Clone the repository
git clone https://github.com/InfinityXOneSystems/git-ops.git
cd git-ops

# (Optional) Install dependencies if applicable
# npm install
# or
# pip install -r requirements.txt
```

### Basic Usage

```bash
# Run the application
# npm start
# or
# python main.py
```

## 🛠️ For Contributors

### 1. Fork & Clone

```bash
# Fork the repo on GitHub, then:
git clone https://github.com/YOUR_USERNAME/git-ops.git
cd git-ops
git remote add upstream https://github.com/InfinityXOneSystems/git-ops.git
```

### 2. Create a Branch

```bash
git checkout -b feature/my-awesome-feature
```

### 3. Make Changes

Edit files, add features, fix bugs...

### 4. Test Your Changes

```bash
# Run tests
# npm test
# or
# pytest

# Run linting
# npm run lint
# or
# flake8 .
```

### 5. Commit

```bash
git add .
git commit -m "feat: add my awesome feature"
```

Follow [Conventional Commits](https://www.conventionalcommits.org/):
- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation changes
- `style:` - Code style changes
- `refactor:` - Code refactoring
- `test:` - Test changes
- `chore:` - Build/tooling changes

### 6. Push & PR

```bash
git push origin feature/my-awesome-feature
```

Then open a Pull Request on GitHub!

## 🔧 For Maintainers

### Setting Up Automation

1. **Enable GitHub Actions**
   - Go to Settings → Actions → Allow all actions

2. **Configure Branch Protection**
   - Protect `main` branch
   - Require PR reviews
   - Require status checks (CI, CodeQL)

3. **Set Up Secrets** (if needed)
   - Go to Settings → Secrets → New repository secret

4. **Enable Dependabot**
   - Already configured in `.github/dependabot.yml`
   - Review and merge Dependabot PRs

5. **Configure Auto-assign**
   - Edit `.github/workflows/auto-assign.yml`
   - Add your GitHub username

### Creating a Release

```bash
# Tag the release
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin v1.0.0
```

The release workflow will automatically:
- Build artifacts
- Generate changelog
- Create GitHub release

## 📚 Using This as a Template

Want to use this setup for your own project?

1. **Use as Template**
   - Click "Use this template" on GitHub
   - Create your new repository

2. **Customize**
   - Update `README.md` with your project details
   - Modify workflows in `.github/workflows/`
   - Update `.github/dependabot.yml` for your languages
   - Edit issue templates
   - Customize `CONTRIBUTING.md`

3. **Remove Unneeded Files**
   - Delete workflows you don't need
   - Remove language-specific configs

4. **Update References**
   - Replace `InfinityXOneSystems/git-ops` with your repo
   - Update author information
   - Change license if needed

## 🎯 Common Tasks

### Reporting a Bug

1. Check [existing issues](https://github.com/InfinityXOneSystems/git-ops/issues)
2. Use [bug report template](.github/ISSUE_TEMPLATE/bug_report.md)
3. Provide details: steps, expected vs actual, environment

### Requesting a Feature

1. Check [existing feature requests](https://github.com/InfinityXOneSystems/git-ops/issues?q=label%3Aenhancement)
2. Use [feature request template](.github/ISSUE_TEMPLATE/feature_request.md)
3. Explain the use case and benefits

### Asking Questions

1. Check [documentation](docs/)
2. Search [closed issues](https://github.com/InfinityXOneSystems/git-ops/issues?q=is%3Aissue+is%3Aclosed)
3. Use [question template](.github/ISSUE_TEMPLATE/question.md)
4. Or start a [discussion](https://github.com/InfinityXOneSystems/git-ops/discussions)

### Reporting Security Issues

**DO NOT** open a public issue!

1. Review [Security Policy](SECURITY.md)
2. Use [GitHub Security Advisories](https://github.com/InfinityXOneSystems/git-ops/security/advisories/new)
3. Provide details privately

## 📖 Next Steps

- Read the full [README](../README.md)
- Check out [Contributing Guidelines](../CONTRIBUTING.md)
- Review [Architecture Documentation](ARCHITECTURE.md)
- See [Development Guide](DEVELOPMENT.md)
- Browse [GitHub Tools Reference](GITHUB_TOOLS_REFERENCE.md)

## 💡 Tips

- Star ⭐ the repo to stay updated
- Watch 👁️ for notifications
- Fork 🍴 to contribute
- Share 📢 with others

## ❓ Need Help?

- 📖 [Documentation](docs/)
- 💬 [GitHub Discussions](https://github.com/InfinityXOneSystems/git-ops/discussions)
- 🐛 [Issues](https://github.com/InfinityXOneSystems/git-ops/issues)
- 📧 [Support](../.github/SUPPORT.md)

---

**Welcome aboard! Happy coding! 🚀**
