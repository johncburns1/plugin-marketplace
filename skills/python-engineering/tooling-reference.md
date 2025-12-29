# Python Tooling Reference

Complete command reference and configurations for uv, ruff, mypy, and pytest.

Referenced from [SKILL.md](SKILL.md) for detailed command usage.

## uv

### All Commands

```bash
# Setup and sync
uv sync                    # Install dependencies and create venv
uv init <project>          # Create new project

# Dependency management
uv add <package>           # Add runtime dependency
uv add --dev <package>     # Add development dependency
uv remove <package>        # Remove dependency
uv lock                    # Update lockfile
uv lock --upgrade          # Upgrade all dependencies

# Running commands
uv run <command>           # Run command in venv
uv run python script.py    # Run Python script
uv run pytest              # Run tests
uv run mypy src            # Type check

# Build and publish
uv build                   # Build distribution packages
```

### Configuration in pyproject.toml

```toml
[project]
name = "myproject"
version = "0.1.0"
requires-python = ">=3.13"
dependencies = [
    "requests>=2.31.0",
]

[dependency-groups]
dev = [
    "pytest>=8.0.0",
    "ruff>=0.1.0",
    "mypy>=1.8.0",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

### Lock Files

- **Always commit** `uv.lock` for applications
- **Don't commit** `uv.lock` for libraries

## Ruff

### All Commands

```bash
# Linting
uv run ruff check .                # Check for issues
uv run ruff check --fix            # Auto-fix issues
uv run ruff check --watch          # Watch mode
uv run ruff check --output-format  # Different output formats

# Formatting
uv run ruff format .               # Format code
uv run ruff format --check         # Check without formatting
uv run ruff format --diff          # Show diff
```

### Configuration

```toml
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

[tool.ruff.lint.isort]
known-first-party = ["mypackage"]
```

### Code Style Standards

- **Line length**: 88 characters (Black-compatible)
- **String quotes**: Double quotes
- **Import sorting**: Automatic via isort
- **Trailing commas**: Yes (improves diffs)

## mypy

### All Commands

```bash
# Type checking
uv run mypy src                     # Check source directory
uv run mypy src/mypackage/module.py # Check specific file
uv run mypy --verbose src           # Detailed output
uv run mypy --strict src            # Strictest mode
```

### Configuration

```toml
[tool.mypy]
python_version = "3.13"
strict = true
warn_return_any = true
warn_unused_configs = true
disallow_untyped_defs = true

# Per-module options
[[tool.mypy.overrides]]
module = "tests.*"
disallow_untyped_defs = false
```

### Type Annotation Patterns

**Modern Python Type Syntax** (Python 3.10+):

```python
# Built-in generics (good)
def process(items: list[str]) -> dict[str, int]:
    return {item: len(item) for item in items}

# Old typing module syntax (avoid)
from typing import List, Dict
def process(items: List[str]) -> Dict[str, int]:
    ...
```

**Common Patterns**:

```python
from collections.abc import Callable, Iterable, Sequence
from typing import Protocol, TypeVar

# Generic types
T = TypeVar("T")

def first(items: Sequence[T]) -> T | None:
    return items[0] if items else None

# Protocols for structural typing
class Drawable(Protocol):
    def draw(self) -> None: ...

# Callable types
def retry(func: Callable[[int], str], times: int) -> str:
    ...

# Union types (prefer | over Union)
def parse(value: str | int) -> Result:
    ...
```

## pytest

### All Commands

```bash
# Running tests
uv run pytest                              # Run all tests
uv run pytest tests/test_module.py         # Run specific file
uv run pytest tests/test_module.py::test_x # Run specific test
uv run pytest -k pattern                   # Run matching tests
uv run pytest -v                           # Verbose output
uv run pytest -x                           # Stop on first failure
uv run pytest --lf                         # Run last failed tests
uv run pytest --ff                         # Run failed tests first

# Coverage
uv run pytest --cov=src                    # Basic coverage
uv run pytest --cov=src --cov-report=html  # HTML report
uv run pytest --cov=src --cov-report=term-missing # Show missing lines
```

### Configuration

```toml
[tool.pytest.ini_options]
testpaths = ["tests"]
python_files = ["test_*.py"]
python_classes = ["Test*"]
python_functions = ["test_*"]
addopts = [
    "--strict-markers",
    "--strict-config",
    "--cov=src",
    "--cov-report=term-missing",
    "--cov-report=html",
]

[tool.coverage.run]
source = ["src"]
omit = ["tests/*", "**/__main__.py"]

[tool.coverage.report]
exclude_lines = [
    "pragma: no cover",
    "def __repr__",
    "raise AssertionError",
    "raise NotImplementedError",
    "if __name__ == .__main__.:",
    "if TYPE_CHECKING:",
]
```

### Test Structure Examples

```python
import pytest
from mypackage import calculate_total, Item

def test_calculate_total_empty_list_returns_zero() -> None:
    """Test that empty list returns zero."""
    result = calculate_total([])
    assert result == 0

@pytest.fixture
def sample_items() -> list[Item]:
    """Provide sample items for testing."""
    return [
        Item(name="Widget", price=10.00),
        Item(name="Gadget", price=20.00),
    ]

def test_with_fixture(sample_items: list[Item]) -> None:
    """Test using a fixture."""
    result = calculate_total(sample_items)
    assert result == 30.00
```

### Mocking

```python
from unittest.mock import Mock, patch
import pytest

@pytest.fixture
def mock_api_client() -> Mock:
    """Provide a mocked API client."""
    client = Mock()
    client.get_data.return_value = {"status": "ok"}
    return client

def test_with_mock(mock_api_client: Mock) -> None:
    """Test using a mocked dependency."""
    result = process_data(mock_api_client)
    assert result.success
    mock_api_client.get_data.assert_called_once()

@patch("mypackage.external_service")
def test_with_patch(mock_service: Mock) -> None:
    """Test with patched external service."""
    mock_service.call.return_value = "result"
    result = my_function()
    assert result == "result"
```

## Python Version Features

### Python 3.10+

**Pattern Matching**:
```python
match response.status:
    case 200:
        return parse_success(response)
    case 404:
        return ResourceNotFound()
    case _:
        return UnknownError()
```

**Structural Pattern Matching**:
```python
match point:
    case (0, 0):
        return "Origin"
    case (x, 0):
        return f"X-axis at {x}"
    case (0, y):
        return f"Y-axis at {y}"
    case (x, y) if x == y:
        return f"Diagonal at {x}"
    case (x, y):
        return f"Point at ({x}, {y})"
```

### Python 3.11+

**Exception Groups**:
```python
try:
    ...
except* ValueError as e:
    # Handle group of ValueErrors
    ...
```

## Dependencies Best Practices

### Choosing Dependencies

- Prefer standard library when possible
- Choose mature, well-maintained packages
- Avoid large dependency trees
- Check for Python 3.13 compatibility

### Pinning Strategy

```toml
[project]
# Runtime: Use minimum version constraints
dependencies = [
    "requests>=2.31.0",
]

[dependency-groups]
# Dev dependencies: Can be more flexible
dev = [
    "pytest>=8.0.0",
    "ruff>=0.1.0",
]
```
