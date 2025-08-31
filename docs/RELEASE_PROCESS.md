# Release Process

This document outlines the release process for AlloAI.

## Pre-Release Checklist

Before creating a release, ensure:

- [ ] All tests are passing locally
- [ ] Documentation is up to date
- [ ] CHANGELOG.md has been updated with all changes
- [ ] Version numbers are consistent across all files
- [ ] Code has been reviewed and approved

## Release Steps

### 1. Update Version Numbers

Update version in all relevant files:
- [ ] `alloai/__init__.py`
- [ ] `pyproject.toml`
- [ ] `setup.py` (if not reading from __init__.py)

### 2. Update CHANGELOG.md

- [ ] Move items from "Unreleased" to new version section
- [ ] Add release date (YYYY-MM-DD format)
- [ ] Verify all significant changes are documented
- [ ] Update comparison links at bottom

### 3. Run Final Tests

```bash
# Run full test suite
pytest

# Run with coverage
pytest --cov=alloai

# Check code formatting
black alloai/
flake8 alloai/
mypy alloai/

# Test installation locally
pip install -e .
```

### 4. Test the Package Build

```bash
# Clean previous builds
rm -rf dist/ build/ *.egg-info

# Build the package
python -m build

# Check the built package
twine check dist/*

# Optional: Test install from wheel
pip install dist/alloai-*.whl
alloai examples/example.md
```

### 5. Commit Release Changes

```bash
git add -A
git commit -m "Release version X.Y.Z"
git push origin master
```

### 6. Create and Push Tag

```bash
# Create annotated tag
git tag -a vX.Y.Z -m "Release version X.Y.Z"

# Push tag to GitHub
git push origin vX.Y.Z
```

### 7. Verify GitHub Actions

- [ ] Check that CI/CD pipeline runs successfully
- [ ] Verify tests pass on all platforms
- [ ] Confirm package is published to PyPI

### 8. Create GitHub Release

Release will be created automatically by GitHub Actions.

### 9. Verify PyPI Release

- [ ] Check package at https://pypi.org/project/alloai/
- [ ] Test installation: `pip install alloai==X.Y.Z`
- [ ] Verify package metadata is correct

### 10. Post-Release

- [ ] Update "Unreleased" section in CHANGELOG.md for next version
- [ ] Announce release (if applicable)
- [ ] Close related issues and PRs
- [ ] Update project boards/milestones

## Version Numbering

We follow [Semantic Versioning](https://semver.org/):

- **MAJOR** version (X.0.0): Incompatible API changes
- **MINOR** version (0.Y.0): Add functionality in backwards compatible manner
- **PATCH** version (0.0.Z): Backwards compatible bug fixes

## Hotfix Process

For critical bugs in production:

1. Create hotfix branch from the release tag
2. Fix the issue
3. Update version to X.Y.Z+1
4. Follow steps 2-9 above
5. Merge hotfix back to main
