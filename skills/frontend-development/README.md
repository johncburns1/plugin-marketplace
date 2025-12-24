# Frontend Development

Senior-level React and Vite development standards for building responsive web applications.

## Overview

This skill provides architectural guidance for building modern web applications with React and Vite. It emphasizes simplicity, type safety, responsive design, and industry best practices.

## When This Skill Activates

Claude will use this skill when:
- Building web applications with React
- Making frontend architectural decisions
- Designing component hierarchies
- Implementing responsive/mobile-first design
- Setting up new React projects
- Reviewing frontend code

## Core Technologies

- **React 18+** - Functional components with hooks
- **Vite** - Fast build tool and dev server
- **TypeScript** - Required for type safety
- **React Router** - Client-side routing
- **React Hooks + Context** - State management

## Key Principles

### Simplicity First
Write simple, maintainable code. Avoid premature optimization and over-engineering.

### Mobile-First Responsive
Design for mobile first, progressively enhance for larger screens.

### Type Safety
TypeScript everywhere. Catch bugs at compile time, not runtime.

### Composition Over Complexity
Build complex UIs from small, focused components.

### Test What Matters
Test user behavior and critical paths, not implementation details.

## What's Covered

- Project structure and organization patterns
- Component design principles and anti-patterns
- State management strategy (local → lifted → context)
- Custom hooks patterns
- Responsive design and mobile-first approach
- Performance optimization guidelines
- TypeScript standards and best practices
- Testing strategy (Vitest, React Testing Library)
- API integration architecture
- Accessibility (a11y) requirements
- Security best practices

## Anti-Patterns Avoided

- Prop drilling through multiple levels
- Massive 500+ line components
- Premature state management libraries
- Using array index as keys
- Mutating state directly
- Overusing useEffect

## Philosophy

This skill embodies a senior-level engineering mindset: write for the next developer, delete code aggressively, simple beats clever, and always prioritize accessibility and mobile experience from the start.
