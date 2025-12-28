# Architecture Guide Skill

**Version**: 1.0.0
**Last Updated**: 2025-12-28

## Description

Conversational methodology for creating high-level, one-page system architecture documents (ARCHITECTURE.md files). This skill guides Claude through a structured process to extract and document:

- Functional and non-functional requirements
- Capacity estimations and scaling considerations
- Core entities and their relationships
- Key interfaces and boundaries
- High-level system design with diagrams
- Trade-offs and architectural decisions

## Persona

Claude operates as a seasoned enterprise architect with 25 years of experience at Fortune 500 companies, combined with startup experience. This perspective ensures architectures balance enterprise-grade thinking with pragmatic, ship-fast mentality.

## When Claude Uses This Skill

Claude automatically activates this skill when:

- User wants to create an architecture document
- Designing a new system's structure
- Creating an ARCHITECTURE.md file
- Organizing technical design thinking
- Translating a PRODUCT.md into system design

## Prerequisites

Works best when a PRODUCT.md file exists (created via the product-definition-guide skill). Claude will read it first to understand product context before diving into architecture.

## Output

Generates a concise `ARCHITECTURE.md` file with:

- One-page constraint (digestible in 5 minutes)
- Technology-agnostic patterns (no specific tech choices)
- High-level diagrams (ASCII or Mermaid)
- Clear trade-off documentation
- Balanced consideration of extensibility, maintainability, cost, and speed

## Key Principles

1. **Stay High-Level**: Patterns over products, whiteboard-friendly
2. **Technology-Agnostic**: "Message queue" not "RabbitMQ"
3. **Trade-off Aware**: Every decision has costs—make them explicit
4. **One Page Rule**: If it doesn't fit, cut it

## Full Documentation

See [SKILL.md](./SKILL.md) for complete methodology, conversational framework, and examples.
