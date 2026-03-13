---
name: kiro-spec
description: Use when starting a new feature, bug fix, or meaningful code change that needs structured planning before writing code. Invoke when the user says "I want to build X", "help me plan this", "write a spec for", "plan this feature", or describes a feature idea needing clarity. Also use for vague or complex ideas that need requirements before implementation. Creates structured spec documents (requirements → design → tasks) through conversation.
---

# Kiro Spec: Requirements-First Development

## Overview

Structure before code. This skill guides you through conversational requirements gathering, then produces three linked markdown documents that enforce discipline before implementation begins.

**Three documents, three concerns:**
- `requirements.md` — **What** needs to be built (user stories + acceptance criteria)
- `design.md` — **How** it will be built (components, data model, integrations)
- `tasks.md` — **Steps** to build it (numbered tasks with requirement traceability)

## Conversation Flow

Work through three phases adaptively — stop asking when you have enough clarity.

### Phase 1: Requirements Elicitation

Ask **one question at a time** until you understand:
- Who are the users and what are they trying to do?
- What does success look like? (acceptance criteria)
- What are the constraints? (tech stack, performance, security, etc.)
- What's out of scope?

**Stop when:** You can write a clear user story with measurable acceptance criteria.

**For simple features:** 2-3 questions may be enough. Don't over-interrogate.

Present the draft requirements doc. **Confirm before writing the file.**

### Phase 2: Design

Ask what's needed based on complexity:
- What are the key components or modules?
- How does data flow? What's the data model?
- What integrations or dependencies are involved?
- How should errors be handled?
- Any performance, security, or scalability considerations?

**Skip questions** that don't apply to the feature.

Present the design draft. **Confirm before writing the file.**

### Phase 3: Task Breakdown

Derive numbered tasks from the design. Each task must:
- Reference the requirement numbers it satisfies
- Have checkbox sub-tasks at the right granularity (not 2-minute steps)
- Be ordered by dependency

Present the tasks. **Confirm before writing the file.**

## Output Files

```
docs/specs/<feature-name>/
├── requirements.md
├── design.md
└── tasks.md
```

Use kebab-case for `<feature-name>` (e.g., `user-notifications`, `search-performance`).

## Document Formats

### requirements.md

```markdown
# Requirements: <Feature Name>

## Overview
[1-2 sentences describing the feature and its purpose]

## Requirements

### 1. [Requirement Group Name]
**User Story:** As a [user type], I want [goal] so that [reason].

#### Acceptance Criteria
- 1.1. [Specific, testable criterion]
- 1.2. [Specific, testable criterion]

### 2. [Next Requirement Group]
**User Story:** As a [user type], I want [goal] so that [reason].

#### Acceptance Criteria
- 2.1. [Criterion]
```

### design.md

```markdown
# Design: <Feature Name>

## Overview
[1-2 sentences on the technical approach]

## Architecture

### Components
[Key components and their responsibilities]

### Data Model
[Schema or data structures if relevant]

### Integrations
[External services, APIs, or internal modules]

## Technical Decisions
[Key decisions made and why]

## Error Handling
[How failures are handled]

## Out of Scope
[Explicitly what this design does NOT cover]
```

### tasks.md

```markdown
# Tasks: <Feature Name>

## Tasks

- [ ] 1. [Task Title]
  - [ ] 1.1. [Sub-task description]
  - [ ] 1.2. [Sub-task description]
  _Requirements: 1.1, 1.2_

- [ ] 2. [Task Title]
  - [ ] 2.1. [Sub-task description]
  _Requirements: 2.1, 2.2_
```

## Key Principles

**One question at a time.** Don't present a list of 8 questions. Ask the most important one, wait for the answer, then ask the next.

**Adaptive depth.** A simple CRUD feature needs 3-5 minutes of discussion. A distributed system feature may need 20. Let complexity drive depth.

**Confirm before writing.** Always present a draft and get explicit confirmation before writing any file to disk.

**Traceability is mandatory.** Every task in `tasks.md` must reference requirement numbers. If a task doesn't map to a requirement, either add the requirement or remove the task.

**Separation of concerns.** Requirements = what the user needs. Design = how the system works. Tasks = what to build. Never mix these.

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Asking all questions at once | One question, wait, then next |
| Writing files without confirmation | Always show draft first, ask "Does this look right?" |
| Tasks without requirement references | Add `_Requirements: X.Y_` to every task |
| Acceptance criteria that can't be tested | Rewrite as observable behavior: "The system returns X when Y" |
| Design before requirements are confirmed | Complete Phase 1 fully before starting Phase 2 |
| Over-engineering simple features | Match document depth to feature complexity |
