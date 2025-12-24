# Backend Development

Senior-level serverless backend architecture and API design standards. Language-agnostic guidance for building secure, scalable systems.

## Overview

This skill provides architectural guidance for building modern serverless backend systems. It emphasizes security, proper API layer separation, and industry best practices applicable to any backend language (Python, TypeScript, Go, etc.).

## When This Skill Activates

Claude will use this skill when:
- Designing backend architecture
- Building REST APIs
- Implementing authentication/authorization
- Designing database schemas
- Making security decisions
- Setting up serverless functions
- Integrating LLM services

## Core Technologies

- **Serverless Functions** - AWS Lambda, Vercel, Cloudflare Workers, Supabase
- **Supabase** - PostgreSQL, auth, storage (recommended for quick projects)
- **Anannas.ai** - LLM gateway for provider abstraction (recommended)
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
- LLM integration patterns (Anannas.ai)
- Performance optimization strategies
- Anti-patterns to avoid

## Philosophy

This skill embodies a senior-level engineering mindset: security is not optional, fail closed by default, type safety end-to-end, monitor everything, and simple beats clever. Build systems that are secure, maintainable, and scalable without unnecessary complexity.
