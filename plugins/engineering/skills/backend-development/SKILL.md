---
name: backend-development
description: Senior-level backend architecture and API design standards for serverless applications. Use when designing backend systems, APIs, databases, implementing authentication/authorization, or making backend architectural decisions. Language-agnostic guidance.
---

# Backend Development - Serverless Architecture & API Design

Senior-level architectural guidance for building secure, scalable serverless backend systems. Language-agnostic principles applicable to any backend language (Python, TypeScript, Go, etc.).

## When to Activate

Activate this skill when the user:

- Is designing backend architecture or API structure
- Needs guidance on REST API design and conventions
- Is implementing authentication or authorization
- Is designing database schemas or migrations
- Needs security guidance (validation, secrets, rate limiting)
- Is setting up serverless functions
- Is integrating LLM services into a backend

## Core Philosophy

**Secure by default, simple by design**. Always enforce proper API boundaries—frontend never queries database directly. Build systems that are easy to reason about and hard to misuse.

## Suggested Stack (Opinionated Starter)

For rapid prototyping without special requirements. Evaluate alternatives based on project needs:

- **Supabase** - PostgreSQL, auth, storage, edge functions (good for quick starts)
- **LLM Gateway** - Use a provider abstraction layer to switch between OpenAI, Anthropic, etc. without code changes
- **Serverless Functions** - AWS Lambda, Vercel, Cloudflare Workers, Supabase Edge Functions

**Language choice**: Project-by-project basis. These patterns apply to Python, TypeScript, Go, etc.

## Serverless Architecture

**Frontend → Serverless API → Database** (mandatory separation)

**Serverless Benefits**: No server management, auto-scaling, pay per use
**Trade-offs**: Cold starts, platform lock-in, stateless design required

**Why API Layer**:

- Centralized business logic and security enforcement
- Database refactoring doesn't break frontend
- API aggregates from multiple sources
- Easier caching, validation, transformation

## API Design

### RESTful Conventions

```text
GET    /api/v1/users          # List
GET    /api/v1/users/:id      # Get one
POST   /api/v1/users          # Create
PATCH  /api/v1/users/:id      # Update
DELETE /api/v1/users/:id      # Delete
```

**Principles**:

- Resource-based URLs (nouns, not verbs)
- HTTP methods define actions
- Plural names (`/users`)
- Version from day one (`/v1/`)
- Nested for relationships (`/users/:id/posts`)

### Response Format

**Success**:

```json
{
  "data": { ... },
  "meta": { "page": 1, "total": 100 }
}
```

**Error**:

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid email",
    "details": { "email": "Must be valid" }
  }
}
```

### HTTP Status Codes

- `200` OK | `201` Created | `204` No Content
- `400` Bad Request | `401` Unauthorized | `403` Forbidden | `404` Not Found
- `429` Rate Limited | `500` Server Error

## Authentication & Authorization

### Authentication (Who are you?)

**Use Proven Systems**: Supabase Auth, Auth0, AWS Cognito (never roll your own)

**Session Management**:

- httpOnly cookies (web) or JWTs (mobile/SPA)
- Short-lived access tokens (15min-1hr) + refresh tokens
- bcrypt/argon2 for password hashing

### Authorization (What can you do?)

**Enforce in API Layer** (primary security):

1. Extract and validate auth token
2. Check user permissions for operation
3. Perform operation if authorized
4. Return 403 if denied

**Patterns**:

- **RBAC** (Role-Based): User roles → permissions
- **ABAC** (Attribute-Based): Check user/resource attributes
- **RLS** (Row Level Security): Database-level defense in depth

**Rule**: Fail closed (deny unless explicitly allowed)

## Database Design

### Schema Principles

- **Normalize first** (avoid duplication), denormalize strategically for read-heavy patterns
- **Constraints**: Foreign keys, unique, NOT NULL, CHECK
- **Migrations always** (never manual schema changes)
- **Generate types** from schema for type safety

### Indexing

**Add indexes for**:

- WHERE clauses
- JOIN columns
- ORDER BY columns
- High cardinality columns

**Skip indexes for**: Small tables, write-heavy columns, low cardinality

### Type Generation

Generate TypeScript/Python types from database schema:

- Supabase: `supabase gen types typescript`
- Benefits: Type safety, catch mismatches at compile time, self-documenting

## Security Best Practices

### Secrets & Credentials

- Store in environment variables (never commit to git)
- Different secrets per environment
- Never expose to frontend (service keys, DB credentials)
- Use secret management services in production

### Input Validation

- Validate everything on backend (never trust client)
- Use schema validation libraries (Zod, Pydantic)
- Whitelist allowed values
- Sanitize before database operations

### Protection Patterns

- **SQL Injection**: Parameterized queries only (no string concatenation)
- **CSRF**: httpOnly cookies + CSRF tokens for state changes
- **CORS**: Whitelist frontend origin (no `*` in production)
- **Rate Limiting**: Prevent abuse (different limits per endpoint)
- **XSS**: Sanitize output, Content Security Policy

## Error Handling

**In Every Function**:

- Top-level error handler
- Log with context (request ID, user ID, timestamp)
- Return user-friendly messages
- Never expose stack traces to users
- Use error monitoring (Sentry, Rollbar)

**What to Log**: Error, stack trace, request ID, user ID, timestamp
**What NOT to Log**: Passwords, tokens, PII, credit cards

## Serverless Function Best Practices

### Design Principles

- **Single responsibility** - One function, one job (under 300 lines)
- **Stateless** - No in-memory state (use database/cache)
- **Idempotent** - Safe to retry
- **Cold start optimization** - Minimize dependencies, lazy load

### Testing

- Unit tests: Business logic
- Integration tests: Database operations
- E2E tests: API endpoints
- Mock database for unit tests, use test DB for integration

## LLM Integration

**Why Use a Gateway/Abstraction Layer**:

- Switch providers (OpenAI, Anthropic, etc.) without code changes
- Automatic fallback, cost tracking, rate limiting
- Centralized configuration and monitoring

**Best Practices**:

- Keep API keys server-side only
- Stream responses for better UX
- Handle timeouts appropriately (30-60s typical)
- Rate limit per user
- Cache common responses when appropriate
- Monitor token usage and costs

## Performance

### Database

- Index slow queries
- Connection pooling
- Cache read-heavy queries (Redis)
- Pagination for large results
- Avoid N+1 queries (use JOIN)

### API

- Enable gzip/brotli compression
- HTTP caching headers
- Pagination (20-100 items per page)
- Measure before optimizing

## Monitoring

**Key Metrics**:

- Request rate, response time (p50, p95, p99), error rate
- Cold start frequency
- LLM token usage/cost
- Database query performance

**Logging**:

- JSON format, structured
- Include request ID for tracing
- Log levels: DEBUG, INFO, WARN, ERROR
- Centralized logging service

## Anti-Patterns to Avoid

### Architecture

- ❌ Frontend queries database directly
- ❌ Business logic in frontend
- ❌ No API versioning
- ❌ Monolithic serverless functions

### Security

- ❌ Rolling your own auth/crypto
- ❌ Trusting client-side validation/authorization
- ❌ No rate limiting
- ❌ Secrets in code or git
- ❌ Exposing stack traces to users

### Database

- ❌ No foreign keys or constraints
- ❌ Manual schema changes
- ❌ No indexes on query columns
- ❌ N+1 queries

### API

- ❌ Inconsistent response formats
- ❌ No pagination
- ❌ Wrong HTTP status codes
- ❌ No input validation

## Related Skills

- [engineering-standards](../engineering-standards/SKILL.md) - Foundational principles (simplicity, TDD, architecture)
- [frontend-development](../frontend-development/SKILL.md) - Frontend that consumes this API layer

## Backend-Specific Mindset

In addition to the universal principles in [engineering-standards](../engineering-standards/SKILL.md#engineering-mindset):

1. **Security is not optional** - Build secure from day one
2. **API layer always** - Frontend never queries database directly
3. **Fail closed** - Deny unless explicitly allowed
4. **Defense in depth** - Multiple security layers
5. **Validate everything** - Never trust client input
6. **Migrations always** - Never manual schema changes
7. **Monitor everything** - Can't fix what you can't measure
