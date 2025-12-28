# Technical Project Manager Skill

**Version**: 1.0.0
**Last Updated**: 2025-12-25

## Description

Principal-level technical project management skill that transforms product requirements into well-organized GitHub issues and milestones. Creates industry-standard, modular work items optimized for junior developer consumption.

## Key Features

- **Small, focused issues** - Maximum 2 story points per issue
- **Consistent templates** - Every issue follows the same structure
- **Milestone organization** - Groups related work into logical phases
- **Context-aware** - Reads PRODUCT.md, ARCHITECTURE.md, and existing issues
- **Iterative approach** - Creates issues incrementally, not all at once
- **gh CLI integration** - Uses GitHub CLI for all operations

## When This Skill Activates

Activate this skill when the user:

- Wants to plan engineering work
- Needs to create GitHub issues from product requirements
- Wants to break down features into tasks
- Needs to organize work into milestones
- Wants to review and plan remaining project work

## Prerequisites

- `gh` CLI installed and authenticated
- `PRODUCT.md` file in project root (use `product-definition-guide` skill to create)
- `ARCHITECTURE.md` file in project root (recommended)

## Issue Template

Every issue created follows this structure:

- **Problem Statement** - What problem does this solve?
- **Feature** - Which PRODUCT.md feature this belongs to
- **Description** - Detailed implementation context
- **Acceptance Criteria** - Specific, testable checkboxes
- **Technical Notes** - Architecture references, patterns to follow
- **Dependencies** - Issues that must be completed first
- **Out of Scope** - What this issue does NOT cover

## Workflow

1. Reads PRODUCT.md and ARCHITECTURE.md for context
2. Queries open/closed GitHub issues for existing work
3. Identifies gaps and work to be planned
4. Creates milestones for larger efforts
5. Creates small, focused issues (max 2 story points)
6. Confirms with user and iterates

## Story Point Guidelines

| Points | Scope |
|--------|-------|
| 1 | Single file, simple function, config update |
| 2 | 2-3 files, new component, API endpoint |
| >2 | Break it down further! |

## Full Documentation

See [SKILL.md](./SKILL.md) for complete methodology, templates, and examples.
