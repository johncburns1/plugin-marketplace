# Python Project Setup Guide

Complete guide to project structure, configuration files, and setup procedures.

## Project Structure

### Standard Layout (src/ layout)

**Use src/ layout** (not flat layout):

```text
project/
├── src/
│   └── mypackage/
│       ├── __init__.py
│       ├── __main__.py
│       └── module.py
├── tests/
│   ├── __init__.py
│   └── test_module.py
├── .pre-commit-config.yaml
├── pyproject.toml
├── README.md
└── uv.lock
```

**Why src/ layout?**:

- Prevents accidental imports from development directory
- Forces proper installation before testing
- Industry standard for modern Python projects

### Package Layout

```python
# src/mypackage/__init__.py
"""Package documentation."""
from mypackage.module import public_function

__all__ = ["public_function"]
__version__ = "0.1.0"
```

## Complete pyproject.toml

### Minimal Configuration

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "myproject"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
authors = [
    { name = "Your Name", email = "your.email@example.com" }
]
dependencies = []

[tool.hatch.build.targets.wheel]
packages = ["src/mypackage"]

[dependency-groups]
dev = [
    "mypy>=1.8.0",
    "pytest>=8.0.0",
    "pytest-cov>=5.0.0",
    "ruff>=0.1.0",
    "pre-commit>=3.0.0",
]

# Ruff configuration
[tool.ruff]
line-length = 88
target-version = "py313"
src = ["src"]

[tool.ruff.lint]
select = [
    "E",   # pycodestyle errors
    "W",   # pycodestyle warnings
    "F",   # pyflakes
    "I",   # isort
    "UP",  # pyupgrade
    "B",   # flake8-bugbear
]

[tool.ruff.format]
quote-style = "double"

# Pytest configuration
[tool.pytest.ini_options]
testpaths = ["tests"]
addopts = ["--cov=src", "--cov-report=term-missing"]

# Coverage configuration
[tool.coverage.run]
source = ["src"]
omit = ["*/tests/*"]

# Mypy configuration
[tool.mypy]
python_version = "3.13"
strict = true
warn_unused_configs = true
disallow_untyped_defs = true
```

## CLI Entry Points

### Defining Entry Points

```toml
[project.scripts]
myapp = "mypackage.__main__:main"
```

### CLI Implementation

```python
# src/mypackage/__main__.py
import sys
from mypackage.cli import main

if __name__ == "__main__":
    sys.exit(main())
```

## Pre-commit

### Setup

```bash
# Install hooks
uv run pre-commit install

# Run manually on all files
uv run pre-commit run --all-files

# Update hooks to latest versions
uv run pre-commit autoupdate
```

### Configuration (.pre-commit-config.yaml)

```yaml
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.1.0
    hooks:
      - id: ruff
        args: [--fix]
      - id: ruff-format

  - repo: https://github.com/pre-commit/mirrors-mypy
    rev: v1.8.0
    hooks:
      - id: mypy
        additional_dependencies: [types-requests]
        args: [--strict]

  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.5.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-added-large-files
      - id: check-json
      - id: check-toml
```

## Initial Project Setup

### Step 1: Create Project

```bash
# Create new project with uv
uv init my-project
cd my-project
```

### Step 2: Set Up Structure

```bash
# Create src/ layout
mkdir -p src/myproject tests

# Create initial files
touch src/myproject/__init__.py
touch src/myproject/__main__.py
touch tests/__init__.py
touch tests/test_example.py
```

### Step 3: Configure pyproject.toml

Copy the complete pyproject.toml example above, adjusting:

- `name`, `description`, `authors`
- `packages` path in hatch configuration
- Dependencies as needed

### Step 4: Install Dependencies

```bash
# Sync all dependencies and create venv
uv sync

# Install pre-commit hooks
uv run pre-commit install
```

### Step 5: Verify Setup

```bash
# Run all checks
uv run ruff check .
uv run ruff format .
uv run mypy src
uv run pytest
```

## Module Organization Best Practices

### Flat vs Nested

**Flat** (for small projects):

```text
src/mypackage/
├── __init__.py
├── models.py
├── services.py
└── utils.py
```

**Nested** (for larger projects):

```text
src/mypackage/
├── __init__.py
├── domain/
│   ├── __init__.py
│   └── models.py
├── services/
│   ├── __init__.py
│   └── api.py
└── adapters/
    ├── __init__.py
    └── database.py
```

### **init**.py Best Practices

```python
# Good: Explicit public API
from mypackage.models import User, Order

__all__ = ["User", "Order"]
__version__ = "0.1.0"

# Avoid: Star imports
from mypackage.models import *  # Don't do this
```

## README Template

```markdown
# Project Name

Brief description of what this project does.

## Installation

\`\`\`bash
uv sync
\`\`\`

## Usage

\`\`\`python
from mypackage import main_function
main_function()
\`\`\`

## Development

\`\`\`bash
# Run tests
uv run pytest

# Type check
uv run mypy src

# Lint and format
uv run ruff check --fix
uv run ruff format
\`\`\`

## License

[Your License]
```

## Quick Reference

### Essential Commands After Setup

```bash
# Development cycle
uv run ruff check --fix  # Lint and fix
uv run ruff format       # Format
uv run mypy src          # Type check
uv run pytest            # Test

# Pre-commit
uv run pre-commit run --all-files

# Build
uv build
```
