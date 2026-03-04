---
name: plan-review
description: Critical implementation plan review. Evaluates plans for completeness, interface contract precision, and simplicity before implementation begins. Used by the plan-reviewer agent.
user-invocable: false
---

# Plan Review

Critical evaluation of implementation plans before a line of code is written.

## Core Principle

**A vague plan produces vague code. A plan with incomplete contracts produces incompatible parallel implementations.**

The purpose of plan review is to ensure two agents working in isolation can independently produce code that is correct and interoperable.

## Review Dimensions

### 1. Scope Clarity

The plan must state unambiguously:
- What is being built (one sentence)
- What is explicitly NOT in scope
- Who or what consumes the output

### 2. Interface Contracts

This is the most critical dimension. Every interface must specify:
- Name and purpose
- Parameter names, types, and constraints
- Return type and shape
- Error types and the conditions that trigger them
- Side effects

A contract is complete when two developers can implement it independently and produce compatible code.

**Incomplete contract (bad)**:
```
createUser(userData) → User
```

**Complete contract (good)**:
```
createUser(userData: CreateUserInput): Promise<User>
  Input:
    - userData.email: string, valid email format, must be unique
    - userData.name: string, 1–100 characters
  Returns:
    - User: { id, email, name, createdAt }
  Throws:
    - ValidationError if input is invalid
    - ConflictError if email already exists
```

#### Failure Mode Completeness Checklist

Before approving any interface contract, verify:

- [ ] Every status code or error type the interface can return is explicitly named in the plan
- [ ] Each named error type has a corresponding acceptance criterion that maps to a test case
- [ ] For changes that replace or remove an existing interface: at least one test verifies the old interface is no longer reachable (route deregistration, removed export, etc.)

### 3. Simplicity

Evaluate every element of the plan against YAGNI and KISS:
- Is this step required to satisfy a stated acceptance criterion?
- Could the same outcome be achieved with fewer components?
- Is any abstraction being added preemptively?

### 4. Acceptance Criteria Quality

Each criterion must be:
- **Specific**: references concrete values or behaviors
- **Testable**: can be verified by an automated test
- **Unambiguous**: only one interpretation possible

**Vague criterion (bad)**:
> Users can log in

**Specific criterion (good)**:
> Given a valid email/password, `POST /auth/login` returns `200` with `{ token: string, expiresAt: ISO8601 }`
> Given an invalid password, `POST /auth/login` returns `401` with `{ error: "InvalidCredentials" }`

#### Route / Handler Impact Analysis (required for plans that add, remove, or rename routes or handlers)
- [ ] List every route or handler group being added, removed, or renamed (by name and path prefix)
- [ ] Confirm no path prefix conflicts with existing registrations
- [ ] Confirm new routes/handlers are registered in the application's entry point (not just defined)

#### File Move / Delete Enumeration (required for structural refactors)
- [ ] List every file being moved: source path → destination path
- [ ] List every file being deleted (not moved)
- [ ] Confirm any module re-exports are updated so existing consumers are not broken
- [ ] Confirm no dead file copies will remain after the refactor

### 5. Refactor Safety (required for plans that move or reorganize files)
- [ ] Source and destination paths are explicit (no ambiguous "move to X")
- [ ] Module export updates at the destination are enumerated
- [ ] Application entry-point registration changes are enumerated (added, removed, renamed handlers)
- [ ] No dead file copies will remain (explicit deletion steps included)

## Output

After review, produce:
1. The **amended plan** with all gaps filled inline
2. A **review summary** noting what was changed and why
3. A **verdict**: APPROVED or NEEDS REVISION

## Reference

The simplicity criteria (KISS, YAGNI) and foundational engineering principles applied in this review are defined in the `engineering-standards` skill. When evaluating a plan's complexity or acceptance criteria quality, consult that skill for the full decision framework and principle definitions.
