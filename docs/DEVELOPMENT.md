# Development Guide

This document contains technical details for developers working on AlloAI.

## Architecture Overview

AlloAI consists of three main components:

### 1. Parser (`alloai/parser.py`)
- Extracts code blocks and text instructions from markdown files
- Uses regex patterns to identify code blocks with optional language tags
- Returns structured list of dictionaries with type, content, and language

### 2. Executor (`alloai/execute.py`)
- Maintains persistent Python runtime environment
- Captures stdout from code execution
- Builds context for LLM including:
  - Previous code block
  - Console output
  - Current variable state
- Executes LLM-generated code in same environment
- Tracks all executed code for export feature

### 3. CLI (`alloai/cli.py`)
- Argument parsing with argparse
- Environment configuration via dotenv
- Error handling and user feedback
- Verbose mode for debugging

## Code Flow

```
1. User runs: alloai script.md
2. CLI loads environment variables (.env)
3. Parser extracts code blocks and instructions
4. For each part:
   a. If code block: Execute directly
   b. If instruction: 
      - Build LLM prompt with current state
      - Get generated code from LLM
      - Execute generated code
5. Optionally export all executed code to file
```

## Key Design Decisions

### Shared Execution Context
All code (both from markdown and LLM-generated) runs in a single shared Python environment. This allows:
- Variables to persist across blocks
- State to build up naturally
- LLM to access all previous computations

### State Tracking
The executor maintains:
- `shared_scope`: Dictionary serving as global namespace
- `last_execution_output`: Most recent console output
- `last_execution_code`: Most recent code block
- `all_executed_code`: List of all code for export

### LLM Integration
- Supports any OpenAI-compatible API
- Prompt includes full context for accurate code generation
- Automatic cleanup of markdown formatting from LLM responses
- Error handling for API failures

## Testing Strategy

### Unit Tests
Test individual components in isolation:
- Parser: Various markdown formats
- Executor: Code execution and output capture
- CLI: Argument parsing and configuration

### Integration Tests
Test complete workflows:
- End-to-end script execution
- LLM integration (with mocked responses)
- Code export functionality

### Test Coverage Goals
- Minimum 80% code coverage
- 100% coverage for critical paths
- Edge cases and error conditions

## Performance Considerations

### Memory Management
- Variable state string generation uses repr() with fallback
- Large data structures may impact LLM context window
- Consider implementing state size limits

### Execution Safety
- Currently no sandboxing (runs in user's Python environment)
- Future: Consider restricted execution environments
- No timeout on code execution (future enhancement)

## Adding New Features

### 1. Supporting New Languages
To add support for a new language:
- Update parser to recognize language tags
- Implement language-specific executor
- Update shared state serialization
- Add appropriate error handling

### 2. Enhanced LLM Providers
To add a new LLM provider:
- Ensure OpenAI-compatible API or add adapter
- Update configuration options
- Add provider-specific error handling
- Document environment variables

### 3. Export Formats
To add new export formats:
- Extend `execute_markdown()` with format parameter
- Implement format-specific code generation
- Add appropriate file headers/footers
- Update CLI arguments

## Debugging Tips

### Verbose Mode
Use `-v` flag to see:
- Parsed markdown structure
- Code being executed
- LLM prompts and responses
- Variable state at each step

### Common Issues

**LLM Not Generating Expected Code**
- Check prompt construction in `execute.py`
- Verify variable state representation
- Test with different models

**State Not Persisting**
- Ensure `shared_scope` is passed correctly
- Check for variable name conflicts
- Verify exec() scope handling

**Export Missing Code**
- Check `all_executed_code` list updates
- Verify comment generation for context
- Test with various instruction lengths

## Code Quality Tools

### Pre-commit Configuration
```yaml
repos:
  - repo: https://github.com/psf/black
    rev: 23.1.0
    hooks:
      - id: black
  - repo: https://github.com/pycqa/flake8
    rev: 6.0.0
    hooks:
      - id: flake8
  - repo: https://github.com/pycqa/isort
    rev: 5.12.0
    hooks:
      - id: isort
```

### Type Hints
Add type hints for better IDE support and type checking:
```python
from typing import List, Dict, Optional

def parse_markdown(md_content: str) -> List[Dict[str, str]]:
    ...

def execute_markdown(
    parsed_markdown: List[Dict[str, str]], 
    output_file: Optional[str] = None
) -> str:
    ...
```

## Building and Distribution

### Package Structure
```
alloai/
├── __init__.py       # Package metadata and version
├── cli.py            # Entry point and CLI
├── parser.py         # Markdown parsing
└── execute.py        # Code execution engine
```

### Version Management
- Version defined in `alloai/__init__.py`
- Also update in `pyproject.toml`
- Follow semantic versioning (MAJOR.MINOR.PATCH)

### Build Process
```bash
# Clean previous builds
rm -rf dist/ build/ *.egg-info

# Build source and wheel distributions
python -m build

# Check package
twine check dist/*
```

### Testing Package Installation
```bash
# Test from TestPyPI
pip install -i https://test.pypi.org/simple/ alloai

# Test from local wheel
pip install dist/alloai-*.whl
```

## Security Considerations

### Code Execution
- Currently executes arbitrary Python code
- No sandboxing or restrictions
- Runs with user's full permissions

### API Keys
- Stored in environment variables
- Never logged or included in output
- Support for .env files for local development

### Future Security Enhancements
- [ ] Sandboxed execution environment
- [ ] Code analysis before execution
- [ ] Configurable execution restrictions
- [ ] Audit logging for executed code

## Monitoring and Analytics

### Logging
- Debug logging with `-v` flag
- Structured logging for production
- No sensitive data in logs

### Error Tracking
- Graceful error handling
- User-friendly error messages
- Stack traces in verbose mode only

## Future Architecture Improvements

### Plugin System
- Allow custom parsers for different formats
- Pluggable LLM providers
- Custom code executors

### Async Execution
- Parallel code block execution where possible
- Streaming LLM responses
- Non-blocking I/O for file operations

### Caching
- Cache LLM responses for identical prompts
- Store parsed markdown structure
- Reuse execution environments

## Resources

- [Python AST](https://docs.python.org/3/library/ast.html) - For safer code execution
- [OpenAI API](https://platform.openai.com/docs) - LLM integration reference
- [Click](https://click.palletsprojects.com/) - Alternative CLI framework
- [Rich](https://github.com/Textualize/rich) - Enhanced terminal output