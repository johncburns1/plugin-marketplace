---
name: python-engineering
description: Python-specific tooling and practices including uv, ruff, mypy, pytest, and modern Python development standards. Use when setting up Python projects or working with Python code.
---

# Python Engineering Standards

Modern Python development practices and tooling conventions for Python 3.13+ projects.

## Core Toolchain

### 1. Package Management: uv

**Why uv**: Fast (10-100x faster than pip), reliable lockfile-based installs, built-in virtual environment management.

**Essential Commands**:

```bash
uv sync           # Install dependencies and create venv
uv add <package>  # Add dependency
uv run <command>  # Run in venv
uv build          # Build distribution
```

For complete command reference and configuration, see [tooling-reference.md](tooling-reference.md#uv).

### 2. Linting & Formatting: Ruff

**Why Ruff**: Extremely fast (10-100x faster), replaces multiple tools (flake8, isort, black, pyupgrade).

**Essential Commands**:

```bash
uv run ruff check --fix  # Lint and auto-fix
uv run ruff format       # Format code
```

**Code Style**:

- Line length: 88 characters
- Double quotes for strings
- Automatic import sorting

For detailed configuration and linting rules, see [tooling-reference.md](tooling-reference.md#ruff).

### 3. Type Checking: mypy

**Why mypy**: Static type checking catches bugs before runtime.

**Requirements**:

- All functions must have type annotations
- Use Python 3.10+ syntax: `list[str]`, `dict[str, int]`
- Prefer `|` over `Union` for union types

**Essential Command**:

```bash
uv run mypy src  # Type check source code
```

For type patterns and advanced configurations, see [tooling-reference.md](tooling-reference.md#mypy).

### 4. Testing: pytest

**Why pytest**: Simple, powerful testing with fixtures and coverage reporting.

**Essential Commands**:

```bash
uv run pytest                    # Run all tests
uv run pytest tests/test_foo.py  # Run specific file
uv run pytest -k pattern         # Run matching tests
```

**Test Naming**: `test_<function>_<scenario>_<expected>`

For fixtures, mocking, and coverage configuration, see [tooling-reference.md](tooling-reference.md#pytest).

### 5. Pre-commit Hooks

**Why pre-commit**: Automated checks before commits prevent issues.

**Setup**:

```bash
uv run pre-commit install              # Install hooks
uv run pre-commit run --all-files      # Run manually
```

For hook configuration, see [project-setup.md](project-setup.md#pre-commit).

## Python Version & Modern Features

**Target**: Python 3.13+

**Use Modern Syntax**:

- Built-in generics: `list[str]`, `dict[str, int]` (not `List`, `Dict`)
- Union operator: `str | int` (not `Union[str, int]`)
- Pattern matching (3.10+)
- Exception groups (3.11+)

## Project Structure

**Use src/ layout**:

```text
project/
├── src/
│   └── mypackage/
│       ├── __init__.py
│       └── module.py
├── tests/
│   └── test_module.py
├── pyproject.toml
└── uv.lock
```

For complete project setup, configuration files, and directory structure, see [project-setup.md](project-setup.md).

## Quick Development Cycle

```bash
# Setup
uv sync

# Development loop
uv run ruff check --fix  # Lint
uv run ruff format       # Format
uv run mypy src          # Type check
uv run pytest            # Test

# Build
uv build
```

## Docstring Style

**Use Google style**:

```python
def calculate_total(items: list[Item], tax_rate: float = 0.0) -> Decimal:
    """Calculate the total price of items including tax.

    Args:
        items: List of items to calculate total for.
        tax_rate: Tax rate as decimal (e.g., 0.08 for 8%).

    Returns:
        Total price including tax.

    Raises:
        ValueError: If tax_rate is negative.
    """
```

**Note**: Don't repeat type information that's already in type hints.

## Further Reading

- [tooling-reference.md](tooling-reference.md) - Complete command reference, configurations, and patterns for uv, ruff, mypy, pytest
- [project-setup.md](project-setup.md) - Project structure, pyproject.toml examples, pre-commit configuration
