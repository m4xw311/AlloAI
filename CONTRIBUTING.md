# Contributing to AlloAI

Thank you for your interest in contributing to AlloAI! We welcome contributions from the community and are grateful for any help you can provide.

## Table of Contents

- [How Can I Contribute?](#how-can-i-contribute)
- [Development Setup](#development-setup)
- [Development Workflow](#development-workflow)
- [Technical Documentation](#technical-documentation)
- [Code Style](#code-style)
- [Testing](#testing)
- [Pull Request Process](#pull-request-process)
- [Reporting Issues](#reporting-issues)
- [Feature Requests](#feature-requests)

## How Can I Contribute?

### Types of Contributions

- **Bug Fixes**: Found a bug? Fix it and submit a PR!
- **Features**: Have an idea for a new feature? Let's discuss it!
- **Documentation**: Help improve our docs or add examples
- **Tests**: Add missing tests or improve existing ones
- **Code Reviews**: Review PRs and provide constructive feedback
- **Issues**: Report bugs or suggest improvements

### First Time Contributors

Looking for a good first issue? Check out issues labeled with `good first issue` or `help wanted`.

## Development Setup

### Prerequisites

- Python 3.8 or higher
- pip and virtualenv
- Git

### Setting Up Your Development Environment

1. **Fork the repository** on GitHub

2. **Clone your fork locally**:
   ```bash
   git clone https://github.com/your-username/AlloAI.git
   cd AlloAI
   ```

3. **Add the upstream repository**:
   ```bash
   git remote add upstream https://github.com/m4xw311/AlloAI.git
   ```

4. **Create a virtual environment**:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

5. **Install in development mode**:
   ```bash
   pip install -e ".[dev]"
   ```

6. **Set up pre-commit hooks** (optional but recommended):
   ```bash
   pre-commit install
   ```

## Development Workflow

1. **Create a feature branch**:
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes**:
   - Write code
   - Add/update tests
   - Update documentation

3. **Run tests locally**:
   ```bash
   pytest
   pytest --cov=alloai  # With coverage
   ```

4. **Check code style**:
   ```bash
   black alloai/
   flake8 alloai/
   mypy alloai/
   ```

5. **Commit your changes**:
   ```bash
   git add .
   git commit -m "Add descriptive commit message"
   ```

   Follow conventional commit format:
   - `feat:` for new features
   - `fix:` for bug fixes
   - `docs:` for documentation changes
   - `test:` for test additions/changes
   - `refactor:` for code refactoring
   - `style:` for formatting changes
   - `chore:` for maintenance tasks

6. **Push to your fork**:
   ```bash
   git push origin feature/your-feature-name
   ```

7. **Create a Pull Request** on GitHub

## Technical Documentation

For detailed technical information about AlloAI's architecture, design decisions, and implementation details, see [DEVELOPMENT.md](docs/DEVELOPMENT.md). This includes:

- Architecture overview and component details
- Code flow and execution model
- Performance considerations
- Security considerations
- Adding new features guide
- Debugging tips and common issues

## Code Style

We use the following tools to maintain code quality:

- **Black** for code formatting
- **Flake8** for linting
- **MyPy** for type checking
- **isort** for import sorting

### Style Guidelines

- Follow PEP 8
- Use meaningful variable and function names
- Add docstrings to all public functions and classes
- Keep functions small and focused
- Write self-documenting code

### Running Code Quality Checks

```bash
# Format code
black alloai/

# Check linting
flake8 alloai/

# Check types
mypy alloai/

# Sort imports
isort alloai/
```

## Testing

### Writing Tests

- Place tests in the `tests/` directory
- Name test files with `test_` prefix
- Use descriptive test names that explain what is being tested
- Aim for high test coverage (>80%)
- Include both unit tests and integration tests

### Running Tests

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=alloai

# Run specific test file
pytest tests/test_parser.py

# Run with verbose output
pytest -v

# Run only failed tests from last run
pytest --lf
```

### Test Structure

```python
def test_function_does_something():
    """Test that function does what it's supposed to do."""
    # Arrange
    input_data = "test"

    # Act
    result = function_under_test(input_data)

    # Assert
    assert result == expected_output
```

## Pull Request Process

1. **Before submitting**:
   - Ensure all tests pass
   - Update documentation if needed
   - Add tests for new functionality
   - Run code quality checks
   - Update CHANGELOG.md if applicable

2. **PR Description**:
   - Clearly describe what changes you've made
   - Reference any related issues (use "Fixes #123" to auto-close issues)
   - Include screenshots for UI changes
   - List any breaking changes

3. **PR Title**:
   - Use a descriptive title
   - Follow conventional commit format

4. **Review Process**:
   - PRs require at least one review before merging
   - Address all feedback constructively
   - Keep PRs focused and reasonably sized
   - Be patient - reviewers are volunteers

5. **After Approval**:
   - Squash commits if requested
   - Ensure CI/CD passes
   - Maintainers will merge your PR

## Reporting Issues

### Before Creating an Issue

- Check if the issue already exists
- Try to reproduce the issue with the latest version
- Collect relevant information (OS, Python version, error messages)

### Creating a Good Issue Report

Include:
- Clear, descriptive title
- Steps to reproduce the issue
- Expected behavior
- Actual behavior
- Error messages (if any)
- Environment details (OS, Python version, AlloAI version)
- Minimal reproducible example

Example:
```markdown
**Description**
When running AlloAI with a markdown file containing [...], the execution fails.

**Steps to Reproduce**
1. Create a file `test.md` with content: [...]
2. Run `alloai test.md`
3. See error

**Expected Behavior**
The script should execute successfully and output [...]

**Actual Behavior**
The script fails with error: [...]

**Environment**
- OS: Ubuntu 22.04
- Python: 3.9.7
- AlloAI: 0.2.0
```

## Feature Requests

We love hearing new ideas! When suggesting a feature:

1. **Check existing issues** to avoid duplicates
2. **Provide context**: Why is this feature needed?
3. **Be specific**: What exactly should the feature do?
4. **Consider alternatives**: Are there other ways to solve the problem?
5. **Think about implementation**: How might this work?

### Feature Request Template

```markdown
**Problem Description**
Describe the problem or limitation you're facing.

**Proposed Solution**
Describe your ideal solution.

**Alternatives Considered**
What other solutions have you considered?

**Additional Context**
Any other context, mockups, or examples.
```

## Release Process (Maintainers Only)

For detailed release instructions, see [docs/RELEASE_PROCESS.md](docs/RELEASE_PROCESS.md).

Quick reference:
1. Update version in `alloai/__init__.py` and `pyproject.toml`
2. Update CHANGELOG.md with release date and changes
3. Run tests and build checks
4. Commit: `git commit -m "Release version X.Y.Z"`
5. Tag: `git tag -a vX.Y.Z -m "Release version X.Y.Z"`
6. Push: `git push origin main --tags`
7. GitHub Actions will automatically publish to PyPI
8. GitHub Actions will automatically create a GitHub release.

## Getting Help

- **Discord/Slack**: [Join our community](#) (if applicable)
- **Discussions**: Use GitHub Discussions for questions
- **Email**: Contact maintainers (see README)

## Recognition

Contributors will be recognized in:
- The README contributors section
- Release notes
- Special thanks in major releases

Thank you for contributing to AlloAI! 🎉
