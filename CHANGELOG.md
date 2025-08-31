# Changelog

All notable changes to AlloAI will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.2.0] - 2025-08-31 - Code Generation and Export

### Added
- Code generation and export feature (`-o` / `--output` flag)
- Generate standalone Python scripts from AlloAI executions
- Include both original and LLM-generated code with helpful comments
- Make exported scripts executable on Unix-like systems
- Support for programmatic code export via Python API
- Better error handling for LLM API calls
- Improved logging with debug mode

### Changed
- Enhanced CLI with more informative output
- Better variable state representation in LLM prompts

### Fixed
- Comment formatting in generated code to prevent syntax errors
- Proper handling of multiline instructions in generated code comments

## [0.1.0] - 2025-08-31 - Initial Release

### Added
- Basic markdown parsing for code blocks and text
- Python code execution with shared runtime
- LLM integration for natural language instructions
- CLI tool for running AlloAI scripts
- PyPI package distribution
- Support for OpenAI-compatible APIs
- Environment variable configuration via .env files
- Verbose mode for debugging
- State preservation across code blocks
- Console output capture and display
- Python API for programmatic use
- Examples directory with sample scripts
- Comprehensive README documentation
- MIT License
- GitHub Actions CI/CD pipeline
- Support for multiple Python versions (3.8+)
- Automated testing and publishing workflows

[Unreleased]: https://github.com/m4xw311/AlloAI/compare/v0.2.0...HEAD
[0.2.0]: https://github.com/m4xw311/AlloAI/compare/v0.1.0...v0.2.0
[0.1.0]: https://github.com/m4xw311/AlloAI/releases/tag/v0.1.0
