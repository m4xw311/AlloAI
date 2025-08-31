# GitHub Actions CI/CD Setup

This directory contains the GitHub Actions workflow configuration for the AlloAI project.

## Overview

The `workflow.yml` file defines a comprehensive CI/CD pipeline that:
- Runs tests on multiple Python versions and operating systems
- Builds the package distribution
- Publishes to TestPyPI (on main branch pushes)
- Publishes to PyPI (on version tags)
- Creates GitHub releases
- Tests the published package

## OIDC Trusted Publishing (No Tokens Required!)

This workflow uses PyPI's **trusted publishing** via OpenID Connect (OIDC), which means:
- ✅ **No API tokens to manage** - More secure and convenient
- ✅ **No secrets in GitHub** - PyPI trusts GitHub directly
- ✅ **Automatic authentication** - GitHub Actions authenticates directly with PyPI
- ✅ **Scoped permissions** - Only this repository can publish to your package

## Setup Instructions

### 1. Configure PyPI Trusted Publisher

#### For PyPI (Production):

1. Log in to https://pypi.org
2. Go to your account settings or create the project first
3. Navigate to the project's "Publishing" settings
4. Add a new trusted publisher:
   - **Publisher**: GitHub
   - **Repository owner**: `m4xw311` (your GitHub username)
   - **Repository name**: `AlloAI`
   - **Workflow name**: `workflow.yml`
   - **Environment**: `pypi` (optional but recommended)

#### For TestPyPI:

1. Log in to https://test.pypi.org
2. Follow the same steps as above
3. Use environment name: `testpypi`

### 2. Configure GitHub Environments (Recommended)

1. Go to your GitHub repository
2. Navigate to Settings → Environments
3. Create two environments:

#### `testpypi` Environment:
- **Protection rules**: None (allow automatic deployment)
- **Deployment branches**: Only from `main` branch

#### `pypi` Environment:
- **Protection rules**:
  - Required reviewers (optional)
  - Wait timer (optional, e.g., 5 minutes)
- **Deployment branches**: Only from protected branches or tags

### 3. First-Time Package Creation

If this is a new package that doesn't exist on PyPI yet:

1. **Option A**: Create the package manually first
   - Go to PyPI and create the project
   - Then add the trusted publisher

2. **Option B**: Use a pending publisher
   - PyPI allows configuring trusted publishers for packages that don't exist yet
   - The package will be created on first publish

## Workflow Triggers

The workflow runs on:

- **Push to main/develop branches**: Runs tests and publishes to TestPyPI
- **Pull requests to main**: Runs tests only
- **Version tags (v*)**: Full release process to PyPI
- **Manual trigger**: Can be run manually from Actions tab

## Jobs Description

### 1. Test Job
- **Matrix Testing**: Tests on Python 3.8-3.12 across Ubuntu, Windows, and macOS
- **Linting**: Runs flake8 for code quality
- **Formatting**: Checks black formatting (non-blocking)
- **Type Checking**: Runs mypy (non-blocking)
- **Unit Tests**: Runs pytest if tests exist
- **CLI Testing**: Verifies CLI installation and basic commands

### 2. Build Job
- Builds wheel and source distributions
- Validates package with twine
- Uploads artifacts for publishing jobs

### 3. Publish to TestPyPI
- **Trigger**: Pushes to main branch
- **Purpose**: Test package distribution before production
- **Authentication**: OIDC trusted publishing (no token needed)
- **URL**: https://test.pypi.org/p/alloai

### 4. Publish to PyPI
- **Trigger**: Version tags (e.g., `v0.1.0`)
- **Purpose**: Production release
- **Authentication**: OIDC trusted publishing (no token needed)
- **URL**: https://pypi.org/p/alloai

### 5. Create GitHub Release
- **Trigger**: After successful PyPI publish
- Automatically creates GitHub release with:
  - Distribution files
  - Auto-generated release notes
  - Installation instructions

### 6. Test Published Package
- **Trigger**: After PyPI publish
- Installs and tests the published package
- Verifies CLI functionality

## Release Process

To release a new version:

1. **Update version numbers** in:
   - `alloai/__init__.py`
   - `pyproject.toml`
   - `setup.py` (if different)

2. **Commit and push changes**:
   ```bash
   git add -A
   git commit -m "Bump version to 0.1.0"
   git push origin main
   ```

3. **Create and push a version tag**:
   ```bash
   git tag -a v0.1.0 -m "Release version 0.1.0"
   git push origin v0.1.0
   ```

4. **Monitor the Actions tab** on GitHub to watch the release process

5. **Verify the release**:
   - Check PyPI: https://pypi.org/project/alloai/
   - Check GitHub Releases page
   - Test installation: `pip install alloai==0.1.0`

## Version Tag Formats

The workflow recognizes these tag formats:
- `v0.1.0` - Standard release
- `v0.1.0rc1` - Release candidate (marked as pre-release)
- `v0.1.0beta1` - Beta release (marked as pre-release)
- `v0.1.0alpha1` - Alpha release (marked as pre-release)

## Python Version Support

The workflow tests against:
- Python 3.8
- Python 3.9
- Python 3.10
- Python 3.11
- Python 3.12

**Note**: Python 3.7 reached end-of-life in June 2023 and is no longer supported.

## Testing Without Publishing

To test the workflow without publishing:

1. **Create a feature branch**:
   ```bash
   git checkout -b test-workflow
   ```

2. **Make changes and push**:
   ```bash
   git push origin test-workflow
   ```

3. **Create a pull request** to main to trigger tests

## Manual Workflow Trigger

You can manually trigger the workflow:

1. Go to Actions tab on GitHub
2. Select "CI/CD Pipeline"
3. Click "Run workflow"
4. Select branch
5. Click "Run workflow" button

## Monitoring and Debugging

### Check Workflow Status
- Go to the Actions tab to see all workflow runs
- Click on a run to see detailed logs
- Check annotations for any warnings or errors

### Common Issues

**"No trusted publisher configured":**
- Ensure you've added the trusted publisher on PyPI/TestPyPI
- Check that repository owner, name, and workflow file match exactly
- Verify environment names match if using environments

**"Insufficient permissions":**
- Check that `id-token: write` permission is set in the workflow
- Ensure the workflow is running from the correct repository

**Package already exists on PyPI:**
- PyPI doesn't allow overwriting versions
- Increment the version number

**Trusted publisher not working:**
- Verify the workflow filename matches exactly (`workflow.yml`)
- Check the environment name matches what's configured on PyPI
- Ensure you're using the correct PyPI URL

## Permissions

The workflow uses these GitHub Actions permissions:
- `contents: read` - Read repository content
- `contents: write` - Create releases and tags
- `id-token: write` - OIDC authentication with PyPI

## Badges

Add these badges to your README.md:

```markdown
[![CI/CD Pipeline](https://github.com/m4xw311/AlloAI/actions/workflows/workflow.yml/badge.svg)](https://github.com/m4xw311/AlloAI/actions/workflows/workflow.yml)
[![PyPI version](https://badge.fury.io/py/alloai.svg)](https://badge.fury.io/py/alloai)
[![Python versions](https://img.shields.io/pypi/pyversions/alloai.svg)](https://pypi.org/project/alloai/)
[![License](https://img.shields.io/pypi/l/alloai.svg)](https://github.com/m4xw311/AlloAI/blob/main/LICENSE)
```

## Security Benefits of OIDC

Using OIDC trusted publishing provides several security benefits:

1. **No Long-Lived Secrets**: No API tokens that could be compromised
2. **Repository Isolation**: Only this specific repository can publish
3. **Workflow Isolation**: Only the specified workflow file can publish
4. **Environment Protection**: Optional environment rules add extra security
5. **Audit Trail**: All publishes are tied to specific GitHub Actions runs
6. **No Secret Rotation**: No tokens to expire or rotate

## Migrating from Token-Based Publishing

If you previously used API tokens:

1. Remove `PYPI_API_TOKEN` and `TEST_PYPI_API_TOKEN` from GitHub Secrets
2. Configure trusted publishers on PyPI and TestPyPI
3. Update workflow to remove `password` field from publish steps
4. Test with a pre-release version first

## Customization

To customize the workflow:

1. **Change Python versions**: Edit the `matrix.python-version` list
2. **Add more OS targets**: Edit the `matrix.os` list
3. **Add more tests**: Create a `tests/` directory with pytest tests
4. **Change deployment conditions**: Modify the `if:` conditions on jobs
5. **Add notifications**: Add steps to send Slack/Discord/email notifications

## Troubleshooting OIDC Issues

### "OIDC token exchange failed"
- Ensure `id-token: write` permission is present
- Check PyPI trusted publisher configuration matches exactly
- Verify you're not running from a fork

### "Environment protection rules not satisfied"
- Check GitHub environment configuration
- Ensure the branch/tag meets environment requirements
- Add required reviewers if configured

### Testing OIDC Configuration
1. First test with TestPyPI before production
2. Use a pre-release version (e.g., `0.1.0rc1`)
3. Check the Actions logs for detailed error messages

## Support

For issues with:
- **OIDC Setup**: Check [PyPI's trusted publishing docs](https://docs.pypi.org/trusted-publishers/)
- **GitHub Actions**: Check [GitHub Actions documentation](https://docs.github.com/en/actions)
- **Package Building**: Check [Python Packaging User Guide](https://packaging.python.org)
