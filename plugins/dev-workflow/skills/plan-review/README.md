# Plan Review

**Version**: 1.0.0
**Last Updated**: 2026-03-03

## Description

Critical implementation plan review skill. Provides the criteria and methodology for evaluating plans before implementation begins.

## When to Use

Used by the `plan-reviewer` agent in step 2 of the dev workflow. Evaluates:

- Scope clarity
- Interface contract completeness
- Simplicity and YAGNI compliance
- Acceptance criteria quality

## Key Insight

Interface contracts must be precise enough that two agents working independently and in parallel will produce compatible, interoperable code. This is the primary gate before parallel implementation begins.
