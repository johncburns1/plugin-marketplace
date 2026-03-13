---
name: quality-gate
description: Run a full quality pipeline on the current changes. Use when the user says "quality gate", "run quality gate", "polish my code", "pre-merge check", "ready to ship?", or "review my changes". Sequentially runs simplify, two rounds of code review (superpowers then dev-workflow), then full validation (build, test, lint, and best-effort e2e).
---

# Quality Gate: Pre-Merge Quality Pipeline

## Overview

A sequential quality pipeline that runs before merging code. Each stage must pass before the next begins. Surface all issues to the user at the end with a clear pass/fail report.

**Five stages, one goal:** ship code you're proud of.

## Pipeline

### Stage 1: Simplify

Invoke the `simplify` skill on the current changes.

- Review all changed files for unnecessary complexity, duplication, or missed reuse opportunities
- Apply fixes inline — don't just report issues
- Complete Stage 1 before proceeding

### Stage 2: Superpowers Code Review

Dispatch a subagent using `subagent_type: superpowers:code-reviewer`.

**Task for subagent:** Review the current changes against the original plan or spec. Check that:
- Implementation matches the stated requirements
- No scope creep or missing requirements
- Code follows the patterns established in this codebase
- No obvious correctness issues

Collect all findings. Proceed to Stage 3 regardless of findings (surface at end).

### Stage 3: Jack-Flow Code Review

Dispatch a subagent using `subagent_type: jack-flow:code-reviewer`.

**Task for subagent:** Review the PR as the final quality gate before merge. Check that:
- Code is production-ready
- Tests are adequate and meaningful
- No security issues
- Documentation is updated where needed

Collect all findings. Proceed to Stage 4 regardless of findings (surface at end).

### Stage 4: Validation

Run all available validation commands. Detect which apply to this project.

#### Build
Detect and run the first that exists:
- `npm run build`
- `make build`
- `uv build`
- `cargo build`
- `python -m build`
- `go build ./...`

#### Lint + Type Check
Detect and run all that apply:
- `ruff check .`
- `mypy .`
- `eslint .`
- `tsc --noEmit`
- `golangci-lint run`
- `cargo clippy`

#### Tests
Detect and run the first that exists:
- `pytest`
- `npm test`
- `cargo test`
- `go test ./...`
- `make test`

#### Best-Effort E2E
Check for local stack scripts and run if found:
- Look for `docker-compose.yml` or `docker compose` support → `docker compose up -d` then smoke tests
- Look for `make dev` or `make start` → run and probe
- Look for a staging environment URL in `.env` or config → run curl/httpx smoke tests against it
- If nothing found: skip and note "No e2e environment detected"

### Stage 5: Summary Report

Output a structured pass/fail report:

```
## Quality Gate Report

### Stage 1: Simplify
✅ PASS — [brief description of changes made, or "No changes needed"]

### Stage 2: Superpowers Code Review
✅ PASS / ⚠️ ISSUES FOUND
[List issues if any]

### Stage 3: Jack-Flow Code Review
✅ PASS / ⚠️ ISSUES FOUND
[List issues if any]

### Stage 4: Validation
- Build:      ✅ PASS / ❌ FAIL / ⏭️ SKIPPED
- Lint:       ✅ PASS / ❌ FAIL / ⏭️ SKIPPED
- Type Check: ✅ PASS / ❌ FAIL / ⏭️ SKIPPED
- Tests:      ✅ PASS / ❌ FAIL / ⏭️ SKIPPED
- E2E:        ✅ PASS / ❌ FAIL / ⏭️ SKIPPED (no environment detected)

### Overall
✅ READY TO MERGE / ❌ ISSUES TO ADDRESS

[If issues: numbered list of action items for the user]
```

## Key Principles

**Sequential, not parallel.** Run each stage in order. Don't skip stages because earlier ones passed.

**Fix what you can, report what you can't.** Stage 1 (simplify) applies fixes directly. Stages 2–3 (code review) surface findings for the user. Stage 4 (validation) reports results.

**Never block on missing tooling.** If a build/lint/test command doesn't exist, mark as SKIPPED — don't fail the pipeline.

**Surface everything at the end.** Don't interrupt the pipeline mid-run to report issues. Collect all findings, then present the full report in Stage 5.

**E2E is best-effort.** A missing e2e environment is not a pipeline failure. Note it and move on.
