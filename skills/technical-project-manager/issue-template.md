# Issue Template

Standard template for creating GitHub issues. See [SKILL.md](SKILL.md) for methodology and workflow.

## Template

```markdown
## Problem Statement

[1-2 sentences describing the problem this issue solves. What pain point or gap does this address?]

## Feature

[Name of the feature from PRODUCT.md this issue belongs to]

## Description

[Detailed description of what needs to be implemented. Include technical context from ARCHITECTURE.md where relevant.]

## Acceptance Criteria

- [ ] [Specific, testable criterion 1]
- [ ] [Specific, testable criterion 2]
- [ ] [Specific, testable criterion 3]

## Technical Notes

[Any relevant technical context, patterns to follow, files to modify, or architectural considerations]

## Dependencies

- [List any issues that must be completed first, or "None"]

## Out of Scope

- [Explicitly list what this issue does NOT cover to prevent scope creep]
```

## Section Guidelines

### Problem Statement

- Keep it to 1-2 sentences
- Focus on the user or business impact
- Answer: "Why does this matter?"

**Good example:**
> Users cannot reset their passwords, forcing them to create new accounts and losing their data history.

**Bad example:**
> We need to add password reset functionality.

### Feature

- Reference the exact feature name from PRODUCT.md
- Use the hierarchy if applicable (e.g., "Core Features > Authentication > Password Reset")
- Helps maintain traceability to product requirements

### Description

- Provide enough context for a junior developer to understand the task
- Reference specific files, patterns, or components from ARCHITECTURE.md
- Include relevant technical decisions or constraints
- Don't over-specify implementation details - leave room for developer judgment

### Acceptance Criteria

- Use checkbox format for easy tracking
- Each criterion should be independently testable
- Be specific and measurable
- Avoid vague criteria like "works correctly" or "handles errors"

**Good criteria:**

- [ ] Password reset email is sent within 30 seconds of request
- [ ] Reset link expires after 24 hours
- [ ] User sees confirmation message after submitting email

**Bad criteria:**

- [ ] Password reset works
- [ ] Emails are sent
- [ ] User experience is good

### Technical Notes

- Reference specific files to modify
- Note patterns to follow from existing codebase
- Mention any architectural constraints
- Include relevant configuration or environment considerations
- Link to related documentation if helpful

### Dependencies

- List issue numbers that must be completed first
- Explain why the dependency exists if not obvious
- Use "None" if the issue can be worked independently
- Minimize dependencies when possible

### Out of Scope

- Explicitly state what this issue does NOT include
- Prevents scope creep during implementation
- Helps developer know when to stop
- Creates natural boundaries for follow-up issues

## Labels to Apply

Every issue should have at least:

1. **One priority label:** `priority: high`, `priority: medium`, or `priority: low`
2. **One type label:** `type: feature`, `type: bug`, `type: tech-debt`, `type: documentation`, or `type: infrastructure`
3. **Component labels as applicable:** `component: frontend`, `component: backend`, `component: database`, `component: devops`

## Example Complete Issue

```markdown
## Problem Statement

Users currently have no way to recover access to their account if they forget their password, resulting in support tickets and lost user engagement.

## Feature

Core Features > Authentication > Password Recovery

## Description

Implement a complete password reset flow including email request, secure token generation, and password update form. The implementation should follow the existing authentication patterns in `src/services/auth/` and use the email service established in `src/services/email/`.

## Acceptance Criteria

- [ ] "Forgot Password" link appears on login page below password field
- [ ] Forgot password page accepts email input and shows confirmation regardless of email existence (security)
- [ ] Valid users receive password reset email within 60 seconds
- [ ] Reset token is cryptographically secure (256-bit) and stored hashed
- [ ] Reset token expires after 24 hours
- [ ] Reset link leads to password update form
- [ ] Password update form validates password strength (min 8 chars, 1 number, 1 special)
- [ ] Successful password reset invalidates token and logs user in
- [ ] Unit tests cover token generation and validation
- [ ] Integration test covers full reset flow

## Technical Notes

- Use `TokenService` pattern from `src/services/auth/token.ts`
- Email template goes in `src/templates/emails/password-reset.html`
- Follow validation patterns in `src/validation/password.ts`
- Store hashed tokens in `password_reset_tokens` table (migration needed)
- Rate limit: max 3 reset requests per email per hour

## Dependencies

- #42 - Email service configuration (needed for sending reset emails)

## Out of Scope

- Account lockout after failed attempts (separate security hardening issue)
- Admin-initiated password resets
- "Remember this device" functionality
- Password history enforcement
```
