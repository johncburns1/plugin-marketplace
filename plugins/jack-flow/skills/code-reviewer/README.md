# code-reviewer

A senior-engineer code review skill for the final quality gate before merge. Evaluates a PR against its original plan across six dimensions: plan alignment, spec↔tests↔code alignment, code quality, language idioms, test quality, and security.

## Usage

Invoke directly as a skill (loads review instructions into current conversation):

```
/jack-flow:code-reviewer
```

Or it is dispatched automatically as an agent by `/jack-flow:quality-gate` (Stage 3).

## What It Reviews

| Dimension | What It Checks |
|-----------|----------------|
| Plan Alignment | Every acceptance criterion met, no scope creep, no missing requirements |
| Spec ↔ Tests ↔ Code | Tests encode spec behavior, no untethered tests, no coincidental passing |
| Code Quality | Simplicity, hexagonal architecture, naming, error handling, file growth |
| Language Idioms | Idiomatic patterns, built-in features used correctly, no anti-patterns |
| Test Quality | No no-ops, right testing level, named correctly, optional mutation testing |
| Security | Input validation, injection risks, auth checks, secrets, error leakage |

## Verdict

- **APPROVE**: All criteria met, no security issues, acceptable quality
- **REQUEST CHANGES**: Any criterion unmet, security issue, critical or important issues present

## Requirements

When using as a skill (not agent), `engineering@jacks-agent-plugins` must also be installed for the engineering standards reference to resolve.
