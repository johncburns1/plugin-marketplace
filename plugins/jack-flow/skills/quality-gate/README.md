# Quality Gate

A sequential pre-merge quality pipeline for Claude Code. Runs simplify → two rounds of code review → full validation (build, test, lint, e2e) and produces a pass/fail report.

## Trigger Phrases

- "quality gate"
- "run quality gate"
- "polish my code"
- "pre-merge check"
- "ready to ship?"
- "review my changes"

## Pipeline Stages

| Stage | What it does |
|-------|-------------|
| **1. Simplify** | Reviews changed code for unnecessary complexity, duplication, and missed reuse. Applies fixes inline. |
| **2. Superpowers Code Review** | Subagent review against the original plan/spec — checks requirements coverage, correctness, and codebase patterns. |
| **3. Dev-Workflow Code Review** | Subagent final quality gate — checks production-readiness, test adequacy, security, and documentation. |
| **4. Validation** | Runs detected build, lint, type check, test, and best-effort e2e commands. |
| **5. Summary Report** | Pass/fail report for each stage with action items for any issues found. |

## Output

```
## Quality Gate Report

### Stage 1: Simplify
✅ PASS — Removed duplicate validation logic in auth.py

### Stage 2: Superpowers Code Review
⚠️ ISSUES FOUND
- Missing error handling for network timeout in api_client.py:42

### Stage 3: Dev-Workflow Code Review
✅ PASS

### Stage 4: Validation
- Build:      ✅ PASS
- Lint:       ✅ PASS
- Type Check: ✅ PASS
- Tests:      ✅ PASS
- E2E:        ⏭️ SKIPPED (no environment detected)

### Overall
❌ ISSUES TO ADDRESS

1. Add timeout error handling in api_client.py:42
```

## Installation

```bash
/plugin install jack-flow@jacks-agent-plugins
```
