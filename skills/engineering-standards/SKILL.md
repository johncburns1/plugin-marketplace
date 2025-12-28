---
name: engineering-standards
description: Core engineering principles including simplicity-first philosophy, test-driven development, and hexagonal architecture patterns. Use when designing new features, refactoring code, or making architectural decisions.
---

# Engineering Standards

Core engineering principles and patterns for building maintainable, testable software.

## Skill Hierarchy

These skills work together in a typical product/engineering workflow:

```text
┌─────────────────────────────────────────────────────────────────┐
│                     PLANNING PHASE                              │
├─────────────────────────────────────────────────────────────────┤
│  product-definition-guide  →  architecture-guide                │
│       (PRODUCT.md)              (ARCHITECTURE.md)               │
│              ↓                        ↓                         │
│              └────────┬───────────────┘                         │
│                       ↓                                         │
│            technical-project-manager                            │
│                (GitHub Issues)                                  │
├─────────────────────────────────────────────────────────────────┤
│                   IMPLEMENTATION PHASE                          │
├─────────────────────────────────────────────────────────────────┤
│                  engineering-standards                          │
│              (foundational principles)                          │
│                       ↓                                         │
│    ┌──────────────────┼──────────────────┐                      │
│    ↓                  ↓                  ↓                      │
│ frontend-       backend-           python-                      │
│ development     development        engineering                  │
│  (React)       (Serverless)        (Tooling)                    │
└─────────────────────────────────────────────────────────────────┘
```

**Planning Phase**: Define what to build, then how to build it, then break into tasks.
**Implementation Phase**: Apply foundational principles through domain-specific skills.

## When to Activate

Activate this skill when the user:

- Is designing new features or system components
- Is refactoring existing code
- Needs guidance on architectural decisions
- Is reviewing code structure or patterns
- Needs to plan a testing strategy
- Asks about TDD, hexagonal architecture, or SOLID principles

## Core Principles

### 1. Simplicity First

**Philosophy**: Simple solutions to complex problems. Less code is better. Only add complexity when truly needed.

**Decision Framework**:

1. Start with the simplest approach that works
2. Add complexity only when requirements demand it
3. Never add complexity in anticipation of future needs
4. If you can't explain it simply, simplify it further

**In Practice**:

- Direct implementations over clever abstractions
- Fewer lines over elaborate patterns
- Built-in features over external dependencies
- Simple conditionals over complex hierarchies

### 2. Test-Driven Development (TDD)

**Core Workflow**:

1. **Write tests first** - Capture desired functionality before implementation
2. **Implement to pass** - Write code to make tests pass
3. **Verify completeness** - Run tests to confirm correctness

**When to use TDD**:

- Core business logic and domain models
- Algorithms and data processing
- API integrations (with mocks)
- Complex functions with edge cases

**When tests can follow**:

- Data models and type definitions
- Configuration files
- Obvious utility functions

**Key Practices**:

- Write tests in batches for related functionality
- Design for testability (small functions, dependency injection)
- Aim for 90% coverage on critical paths
- Test naming: `test_<function>_<scenario>_<expected>`

For detailed test organization, fixtures, and mocking patterns, see [tdd-practices.md](tdd-practices.md).

### 3. Hexagonal Architecture (Ports & Adapters)

**Core Concept**: Isolate business logic from external concerns. Domain code has no dependencies on frameworks or infrastructure.

**Key Layers**:

```text
External Systems (UI, APIs, Databases)
          ↓
     Adapters (implementations)
          ↓
      Ports (interfaces)
          ↓
  Application (use cases)
          ↓
     Domain (business logic)
```

**Essential Rules**:

1. **Dependencies point inward** - Domain depends only on interfaces
2. **Domain models are pure** - No framework dependencies
3. **Repositories translate** - Separate domain and persistence models
4. **Ports define contracts** - Interfaces between layers

**Benefits**:

- Testability: Domain logic tested in isolation
- Flexibility: Swap implementations without changing business logic
- Maintainability: Clear separation of concerns
- Independence: Business logic not tied to frameworks

For detailed patterns, examples, and implementation guidance, see [hexagonal-architecture.md](hexagonal-architecture.md).

### 4. Fundamental Coding Principles

Core principles to apply throughout development:

**SRP (Single Responsibility Principle)**:

- Each function, class, or module should have one reason to change
- If you can describe what it does with "and", it's doing too much
- Cohesive units are easier to test, understand, and modify

**DRY (Don't Repeat Yourself)**:

- Avoid duplicating logic across the codebase
- Extract common patterns into reusable functions
- Balance: Don't abstract too early - wait for the third repetition

**KISS (Keep It Simple, Stupid)**:

- Choose the simplest solution that solves the problem
- Simple code is easier to debug, test, and maintain
- Complexity should come from requirements, not implementation

**YAGNI (You Aren't Gonna Need It)**:

- Don't build features or abstractions for hypothetical future needs
- Add functionality only when actually required
- Over-engineering wastes time and adds maintenance burden

## Code Review Checklist

When reviewing code:

- [ ] Is this the simplest solution?
- [ ] Are tests written (for non-trivial code)?
- [ ] Do tests cover edge cases?
- [ ] Is business logic in domain layer?
- [ ] Do dependencies point inward?
- [ ] Are interfaces used instead of concrete types?
- [ ] Can this be tested in isolation?
- [ ] Is the code self-documenting?

## Quick Reference

**Simplicity**: Direct > Clever
**Testing**: Write tests first for core logic
**Architecture**: Domain → Application → Ports → Adapters → External
**SRP**: One reason to change
**DRY**: Extract common logic, but wait for third repetition
**KISS**: Simplest solution wins
**YAGNI**: Build only what's needed now

## Engineering Mindset

Universal principles that apply across all domains (frontend, backend, infrastructure):

1. **Simple beats clever** - Maintainable over impressive
2. **Optimize when measured** - Profile before optimizing; don't guess at bottlenecks
3. **Write for the next developer** - Code is read 10x more than written
4. **Delete code aggressively** - Dead code is technical debt
5. **Type safety prevents bugs** - Catch errors at compile time, not runtime
6. **Test what matters** - Critical paths and edge cases, not 100% coverage
7. **Dependencies are liabilities** - Each package is a future security/maintenance burden
8. **Design for failure** - Handle errors gracefully; assume things will break
9. **Leverage platforms** - Don't build what you can buy or use from proven tools
10. **Build, measure, learn** - Ship fast, iterate based on real usage

These principles are foundational. Domain-specific skills (frontend-development, backend-development) extend these with specialized guidance.

## Related Skills

This skill provides foundational principles used by:

- [frontend-development](../frontend-development/SKILL.md) - React/Vite web development
- [backend-development](../backend-development/SKILL.md) - Serverless API architecture
- [python-engineering](../python-engineering/SKILL.md) - Python-specific tooling

## Further Reading

- [hexagonal-architecture.md](hexagonal-architecture.md) - Detailed patterns, layer responsibilities, and complete examples
- [tdd-practices.md](tdd-practices.md) - Test organization, naming conventions, fixtures, and mocking strategies
