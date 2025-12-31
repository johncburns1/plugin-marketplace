# Test-Driven Development - Detailed Practices

Comprehensive guide to test organization, naming, fixtures, mocking, and coverage strategies.

Referenced from [SKILL.md](SKILL.md) for detailed implementation guidance.

## Test Organization

### Directory Structure

- Mirror source directory structure
- `tests/test_foo.py` tests `src/foo.py`
- Group related tests in classes when helpful

```text
project/
├── src/
│   └── mypackage/
│       ├── orders.py
│       └── payments.py
└── tests/
    ├── test_orders.py
    └── test_payments.py
```

### Test Naming

**Pattern**: `test_<function>_<scenario>_<expected>`

**Examples**:

- `test_calculate_total_empty_list_returns_zero`
- `test_validate_email_invalid_format_raises_error`
- `test_process_payment_insufficient_funds_returns_failure`
- `test_create_order_valid_items_creates_pending_order`

## Test Types

### Unit Tests (Primary Focus)

- Test single functions in isolation
- Mock external dependencies (APIs, databases, file I/O)
- Fast execution, no network calls
- Use test fixtures for test data

**Example**:

```python
def test_calculate_total_multiple_items_sums_prices() -> None:
    """Test multiple items sums all prices."""
    items = [
        Item(name="Widget", price=10.00),
        Item(name="Gadget", price=20.00),
    ]
    result = calculate_total(items)
    assert result == 30.00
```

### Integration Tests (Broader Scope)

- Test multiple components working together
- May hit real external services (in test environment)
- Slower, more comprehensive
- Test end-to-end workflows

## Test Fixtures

**Purpose**: Reusable test data and setup

```python
import pytest

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

## Mocking

### Using unittest.mock

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

## Coverage Guidance

**Target**: Aim for 90% code coverage on critical paths

**What to focus on**:

- Core business logic
- Complex algorithms
- Error handling paths
- Edge cases and boundary conditions

**What to skip**:

- Framework boilerplate
- Simple getters/setters
- Trivial utility functions

**Remember**: Coverage is a guide, not a goal. Focus on meaningful tests.

## Writing Tests in Batches

**Approach**:

1. Think through all scenarios before implementing
2. Write multiple related tests at once
3. Consider: happy path, edge cases, error conditions, boundary values
4. Implement to make all tests pass

**Example Batch**:

```python
# All tests for calculate_total
def test_calculate_total_empty_list_returns_zero() -> None: ...
def test_calculate_total_single_item_returns_price() -> None: ...
def test_calculate_total_multiple_items_sums_prices() -> None: ...
def test_calculate_total_negative_price_raises_error() -> None: ...
```

## Design for Testability

**Principles**:

- Prefer interfaces over concrete implementations
- Write small, modular functions
- Use functional programming style where appropriate
- Minimize side effects and global state
- Inject dependencies (don't create them inside functions)

**Example**:

```python
# Hard to test - creates its own dependencies
def process_order(order_id: str) -> None:
    db = Database()  # Can't mock this
    order = db.get_order(order_id)
    ...

# Easy to test - dependencies injected
def process_order(order_id: str, db: Database) -> None:
    order = db.get_order(order_id)
    ...
```
