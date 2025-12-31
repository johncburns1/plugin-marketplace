# Hexagonal Architecture - Detailed Guide

Comprehensive patterns and examples for implementing hexagonal architecture (Ports and Adapters) with Domain-Driven Design.

Referenced from [SKILL.md](SKILL.md) for detailed implementation guidance.

## Architecture Layers

### Layer Responsibilities

```text
┌─────────────────────────────────────────┐
│          External Systems               │
│   (UI, APIs, Databases, Services)       │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│           Adapters Layer                │
│  (Concrete implementations of ports)    │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│            Ports Layer                  │
│     (Interfaces and contracts)          │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│      Application Layer (Use Cases)      │
│    (Orchestrates domain operations)     │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│          Domain Layer                   │
│   (Business logic, entities, rules)     │
└─────────────────────────────────────────┘
```

## Domain Models

**Characteristics**:

- Pure business logic, no framework dependencies
- Rich behavior, not just data containers
- Self-validating
- Immutable where possible
- Express ubiquitous language

**Example**:

```python
class Order:
    def __init__(self, items: list[OrderItem]):
        self.items = items
        self.status = OrderStatus.PENDING

    def calculate_total(self) -> Decimal:
        return sum(item.subtotal() for item in self.items)

    def can_be_cancelled(self) -> bool:
        return self.status in [OrderStatus.PENDING, OrderStatus.CONFIRMED]

    def cancel(self) -> None:
        if not self.can_be_cancelled():
            raise OrderCannotBeCancelled(self.status)
        self.status = OrderStatus.CANCELLED
```

## Ports (Interfaces)

### Input Ports (Use Cases)

```python
class PlaceOrderUseCase(Protocol):
    def execute(self, command: PlaceOrderCommand) -> OrderResult:
        ...
```

### Output Ports (Repository interfaces)

```python
class OrderRepository(Protocol):
    def save(self, order: Order) -> None:
        ...

    def find_by_id(self, order_id: OrderId) -> Order | None:
        ...
```

## Adapters

### Repository Adapter

**Key Pattern**: Separate persistence models from domain models

```python
class PostgresOrderRepository:
    # Separate persistence model
    class OrderModel:
        # SQLAlchemy or similar ORM model
        pass

    def save(self, order: Order) -> None:
        # Translate domain model to persistence model
        db_model = self._to_db_model(order)
        session.add(db_model)
        session.commit()

    def find_by_id(self, order_id: OrderId) -> Order | None:
        db_model = session.query(OrderModel).get(order_id)
        # Translate persistence model to domain model
        return self._to_domain_model(db_model)

    def _to_db_model(self, order: Order) -> OrderModel:
        # Translation logic here
        ...

    def _to_domain_model(self, db_model: OrderModel) -> Order:
        # Translation logic here
        ...
```

## Application Layer (Use Cases)

Orchestrates domain operations and coordinates adapters:

```python
class PlaceOrderService:
    def __init__(
        self,
        order_repo: OrderRepository,
        inventory: InventoryService,
        payment: PaymentService
    ):
        self.order_repo = order_repo
        self.inventory = inventory
        self.payment = payment

    def execute(self, command: PlaceOrderCommand) -> OrderResult:
        # Orchestrate domain operations
        order = Order.create(command.items)

        if not self.inventory.check_availability(order.items):
            return OrderResult.insufficient_inventory()

        payment_result = self.payment.charge(order.calculate_total())
        if not payment_result.success:
            return OrderResult.payment_failed()

        self.order_repo.save(order)
        return OrderResult.success(order)
```

## Anti-Patterns to Avoid

**Don't**:

- Put business logic in adapters
- Let domain models depend on frameworks
- Mix persistence concerns with domain logic
- Create anemic domain models (just getters/setters)
- Skip repository translation (persist domain models directly)
- Let domain layer know about external systems

**Do**:

- Keep domain pure and framework-agnostic
- Use repository pattern for all persistence
- Translate between domain and persistence models
- Put business rules in domain models
- Use dependency injection for adapters
- Let application layer orchestrate workflows
