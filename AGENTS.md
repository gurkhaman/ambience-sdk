# Agent Guide for ambience-sdk

This document provides essential information for AI coding agents working on the ambience-sdk codebase.

## Project Overview

- **Name**: ambience-sdk
- **Type**: Python SDK for world-state-aware NPC dialogue
- **Python Version**: 3.9+
- **Package Manager**: uv (v0.9.20+)
- **Build System**: uv_build

## Build, Test, and Lint Commands

### Package Management
```bash
# Install dependencies
uv sync

# Install with all optional dependencies
uv sync --all-extras

# Install in editable mode for development
uv pip install -e .
```

### Running Code
```bash
# Run a Python script
uv run python script.py

# Run a Python module
uv run -m module_name

# Run with specific extras
uv run --extra dev python script.py
```

### Testing
```bash
# Run all tests (when pytest is configured)
uv run pytest

# Run a single test file
uv run pytest tests/test_specific.py

# Run a specific test function
uv run pytest tests/test_specific.py::test_function_name

# Run with verbose output
uv run pytest -v

# Run with coverage
uv run pytest --cov=ambience_sdk --cov-report=html
```

### Linting and Type Checking
```bash
# Run ruff linter (when configured)
uv run ruff check .

# Auto-fix linting issues
uv run ruff check --fix .

# Format code with ruff
uv run ruff format .

# Run mypy type checker (when configured)
uv run mypy src/ambience_sdk

# Run all quality checks
uv run ruff check . && uv run ruff format --check . && uv run mypy src/ambience_sdk
```

### Building
```bash
# Build the package
uv build

# Build specific formats
uv build --sdist
uv build --wheel
```

## Code Style Guidelines

### Imports
- Use absolute imports: `from ambience_sdk.module import function`
- Group imports in order: standard library, third-party, local modules
- One import per line for clarity
- Use `from typing import` for type hints

Example:
```python
import os
from pathlib import Path

from third_party import SomeClass

from ambience_sdk.core import CoreModule
from ambience_sdk.utils import helper_function
```

### Type Hints
- **MANDATORY**: All public functions and methods must have type hints
- Include return type annotations, including `-> None` for void functions
- Use modern type hint syntax (e.g., `list[str]` instead of `List[str]` for Python 3.9+)
- The package supports PEP 561 (py.typed marker is present)

Example:
```python
def process_dialogue(text: str, context: dict[str, str]) -> str:
    """Process dialogue with world state context."""
    return transformed_text

def update_state(state: dict[str, object]) -> None:
    """Update the world state."""
    pass
```

### Naming Conventions
- **Functions/Variables**: `snake_case`
- **Classes**: `PascalCase`
- **Constants**: `UPPER_SNAKE_CASE`
- **Private members**: prefix with single underscore `_private_method`
- **Module names**: lowercase, short, no underscores if possible

### Formatting
- Line length: 88 characters (Black/Ruff default)
- Use double quotes for strings
- 4 spaces for indentation (never tabs)
- Two blank lines between top-level definitions
- One blank line between methods

### Error Handling
- Use specific exception types, avoid bare `except:`
- Create custom exceptions for domain-specific errors
- Include informative error messages
- Use context managers for resource management

Example:
```python
class DialogueError(Exception):
    """Base exception for dialogue processing errors."""
    pass

def parse_dialogue(data: str) -> dict[str, object]:
    """Parse dialogue data."""
    try:
        result = process(data)
    except ValueError as e:
        raise DialogueError(f"Failed to parse dialogue: {e}") from e
    return result
```

### Docstrings
- Use triple double-quotes `"""`
- One-line docstrings for simple functions
- Multi-line format: summary line, blank line, detailed description
- Document parameters and return values for complex functions

Example:
```python
def calculate_response(input_text: str, world_state: dict[str, object]) -> str:
    """Calculate NPC response based on world state.
    
    Args:
        input_text: The player's input dialogue
        world_state: Current state of the game world
        
    Returns:
        Generated NPC response text
        
    Raises:
        DialogueError: If response generation fails
    """
    pass
```

## File Organization

```
ambience-sdk/
├── src/
│   └── ambience_sdk/
│       ├── __init__.py      # Public API exports
│       ├── py.typed         # PEP 561 type hint marker
│       ├── core/            # Core functionality
│       ├── models/          # Data models
│       └── utils/           # Utility functions
├── tests/                   # Test files (mirror src structure)
├── pyproject.toml           # Project configuration
└── README.md               # Project documentation
```

## Development Workflow

1. **Before making changes**: Read relevant code files first
2. **Write type-safe code**: All new code must include type hints
3. **Test your changes**: Write tests for new functionality
4. **Format and lint**: Run ruff before committing
5. **Update docstrings**: Document all public APIs

## Common Patterns

### Adding a new module
1. Create the module file in `src/ambience_sdk/`
2. Add type hints to all functions
3. Create corresponding test file in `tests/`
4. Export public APIs in `__init__.py` if needed

### Making PRs
1. Ensure all tests pass: `uv run pytest`
2. Ensure code is formatted: `uv run ruff format .`
3. Ensure no lint errors: `uv run ruff check .`
4. Update documentation if adding public APIs

## Notes for Agents

- This project uses `uv` not `pip` or `poetry` - always use `uv` commands
- The `py.typed` marker indicates this is a typed package - maintain type safety
- Package uses `uv_build` backend - don't suggest setuptools
- Target Python 3.9+ - use modern syntax but ensure compatibility
- SDK is for NPC dialogue systems - consider gaming/NPC context in implementations
