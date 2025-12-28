# Backend Development

**Version**: 1.0.0
**Last Updated**: 2025-12-28

Senior-level serverless backend architecture and API design standards. Language-agnostic guidance for building secure, scalable systems.

## Overview

This skill provides architectural guidance for building modern serverless backend systems. It emphasizes security, proper API layer separation, and industry best practices applicable to any backend language (Python, TypeScript, Go, etc.).

## When This Skill Activates

Activate this skill when the user:

- Is designing backend architecture
- Is building REST APIs
- Is implementing authentication or authorization
- Is designing database schemas
- Needs guidance on security decisions
- Is setting up serverless functions
- Is integrating LLM services

## Core Technologies

- **Serverless Functions** - AWS Lambda, Vercel, Cloudflare Workers, Supabase Edge Functions
- **Supabase** - PostgreSQL, auth, storage (suggested for rapid prototyping)
- **LLM Gateway** - Provider abstraction layer for switching between LLM providers
- **Language-agnostic** - Apply patterns to Python, TypeScript, Go, etc.

## Key Principles

### API Layer Separation

Frontend never queries database directly. All access through serverless API endpoints.

### Security First

Proper authentication, authorization, input validation, and secrets management from day one.

### Type Safety

Generate types from database schema for compile-time safety across the stack.

### Simple & Scalable

Leverage serverless platforms for auto-scaling without complex infrastructure.

## What's Covered

- Serverless architecture patterns
- RESTful API design and versioning
- Authentication & authorization (RBAC, ABAC, RLS)
- Database design, migrations, and indexing
- Security best practices (validation, secrets, rate limiting)
- Error handling and monitoring
- LLM integration patterns
- Performance optimization strategies
- Anti-patterns to avoid

## Philosophy

Extends the universal principles from [engineering-standards](../engineering-standards/SKILL.md) with backend-specific guidance: security is not optional, API layer always, fail closed, defense in depth, and validate everything. Build systems that are secure, maintainable, and scalable.
