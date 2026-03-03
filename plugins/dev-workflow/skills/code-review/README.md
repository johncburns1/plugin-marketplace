# Code Review

**Version**: 1.0.0
**Last Updated**: 2026-03-03

## Description

Code review skill for evaluating a PR against the original implementation plan as a final gate before merge.

## When to Use

Used by the `code-reviewer` agent in step 5 of the dev workflow. Reviews:

- Plan alignment (every acceptance criterion met, no scope creep)
- Code quality (simplicity, architecture, error handling)
- Test quality (coverage, naming, determinism)
- Security (input validation, secrets, injection risks)

## Output

Produces a structured verdict: APPROVE or REQUEST CHANGES, with specific required changes and optional suggestions.
