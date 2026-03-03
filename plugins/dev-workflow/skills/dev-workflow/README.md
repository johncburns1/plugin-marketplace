# Dev Workflow

**Version**: 1.1.0
**Last Updated**: 2026-03-03

## Description

The primary orchestration command for the full development workflow. Runs the complete pipeline from requirements to merged PR with two human touchpoints.

## When to Use

Invoke with `/dev-workflow [feature description]` to start a new feature implementation session.

## Pipeline

| Step | Agent/Skill | Human? |
|------|-------------|--------|
| 1. Plan | `superpowers:writing-plans` | Provides requirements |
| 2. Plan Review | `plan-reviewer` agent | — |
| 3. Approve Plan | — | **Approves plan → GitHub issue created** |
| 4. Implement | `implementation-agent` (full TDD cycle) | — |
| 5. Code Review | `code-reviewer` agent | — |
| 6. Merge Decision | — | **Reviews findings → merge or send back** |
| 7. Retro | `retro-agent` | — |

## Human Touchpoints

Only two points where human input is required:

1. **Plan approval** — review the amended plan before implementation begins
2. **Merge decision** — review the code review findings before merging

## Usage

```bash
/dev-workflow add user authentication with JWT tokens
```
