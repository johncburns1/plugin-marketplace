---
name: code-review
description: Code review against the original implementation plan. Evaluates plan alignment, code quality, test quality, and security as a final gate before merge. Used by the code-reviewer agent.
user-invocable: false
---

# Code Review

Review the implementation against the original plan as the final quality gate before merge.

## Core Principle

**The plan is the source of truth.** The review determines whether the implementation faithfully realizes the plan with quality, correctness, and security.

## Review Process

1. Read the original GitHub issue (the approved plan) in full
2. Review the PR diff
3. Evaluate each dimension below
4. Produce a structured verdict

## Review Dimensions

### Plan Alignment (highest priority)

Verify the implementation matches the plan exactly:
- Does the implementation satisfy every acceptance criterion?
- Are all interface contracts implemented exactly as specified?
- Is there code that wasn't in the plan (scope creep)?
- Is anything from the plan missing from the implementation?

### Code Quality

- Is the code as simple as possible while meeting requirements?
- Is business logic isolated from infrastructure (hexagonal architecture)?
- Are functions and variables named to reveal intent?
- Is error handling complete for every failure mode in the plan?
- Are there obvious performance issues (N+1 queries, unnecessary allocations)?
- Do patterns follow engineering standards: SRP, DRY, KISS, YAGNI?

### Test Quality

- Does the test suite cover every acceptance criterion?
- Are all failure modes and error types tested?
- Are tests named using `test_<function>_<scenario>_<expected>` convention?
- Is coverage adequate for the new code?
- Are tests independent and deterministic?

### Security

- Is user input validated at system boundaries?
- Are secrets not hardcoded?
- Are there SQL injection, XSS, or command injection risks?
- Is authentication/authorization applied where required by the plan?
- [ ] Auth-related response headers (e.g. `WWW-Authenticate`) are applied consistently across all touched files, not just the primary file changed in the PR
- [ ] All new route/handler groups are registered in the application entry point; no duplicate path prefix registrations exist

## Verdict Criteria

**APPROVE** when:
- All acceptance criteria are met
- No security issues
- Error handling is complete for all failure modes in the plan
- Code quality is acceptable (does not need to be perfect)

**REQUEST CHANGES** when:
- Any acceptance criterion is not met
- Any interface contract deviates from the plan without documented rationale
- Security issues are present
- Critical error handling is missing

## Reference

The principles applied in this review — simplicity-first, hexagonal architecture, SRP, DRY, KISS, YAGNI, and the TDD test quality standards — are defined in the `engineering-standards` skill. Consult that skill when evaluating whether code meets architectural or quality standards, particularly for hexagonal architecture layer boundaries and the code review checklist.

## Output Format

```
## Code Review

### Plan Alignment
[for each criterion: ✓ met or ✗ not met with specific detail]

### Code Quality
[findings]

### Test Quality
[findings]

### Security
[findings]

### Verdict
APPROVE / REQUEST CHANGES

### Required Changes
- [specific change]: [reason — tied to a plan requirement or security issue]

### Optional Suggestions
- [suggestion]: [benefit — these do not block merge]
```
