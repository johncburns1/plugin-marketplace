---
name: frontend-development
description: Senior-level React web development standards. Use when building web applications with React + Vite, designing component architecture, responsive design, or making frontend architectural decisions.
---

# Frontend Development - React & Vite

Senior-level architectural guidance for building responsive web applications with React and Vite.

## When to Activate

Activate this skill when the user:

- Is building web applications with React
- Needs guidance on component architecture or design patterns
- Is implementing state management strategies
- Is designing responsive or mobile-first layouts
- Is setting up a new React + Vite project
- Needs guidance on TypeScript patterns for React
- Is reviewing or refactoring frontend code

## Core Philosophy

**Simplicity over cleverness**. Write code that's easy to understand, maintain, and delete. Avoid premature optimization and over-engineering. Boring, obvious code wins.

## Technology Stack

- **React 18+** with functional components and hooks
- **Vite** for development and production builds
- **TypeScript** required for all projects
- **React Router** for client-side routing
- **React hooks + Context** for state management (no Redux/MobX unless proven necessary)

## Project Structure

```text
src/
├── components/
│   ├── ui/              # Generic reusable components (Button, Input, Card)
│   └── features/        # Feature-specific components (UserProfile, PostList)
├── hooks/               # Custom hooks for logic reuse
├── contexts/            # React contexts for global state
├── lib/                 # API clients, utilities, configuration
├── types/               # TypeScript type definitions
├── pages/               # Route/page components
└── styles/              # Global styles, CSS modules
```

### Alternative: Feature-Based (for larger apps)

```text
src/
├── features/
│   ├── auth/
│   │   ├── components/
│   │   ├── hooks/
│   │   └── types.ts
│   └── dashboard/
│       ├── components/
│       ├── hooks/
│       └── types.ts
├── shared/
│   ├── components/
│   ├── hooks/
│   └── ui/
└── lib/
```

Choose structure based on app size. Type-based is simpler, feature-based scales better.

## Architectural Patterns

### Component Design Principles

1. **Functional components only** - No class components
2. **Named exports** - Better refactoring and IDE support
3. **Single responsibility** - One component does one thing
4. **Composition over props** - Build complex UIs from simple pieces
5. **TypeScript interfaces first** - Define props before implementation
6. **Keep components under 200 lines** - Split if larger

### State Management Strategy

**Local → Lifted → Context → Library** (in that order)

1. **Local state first** (`useState`) - Component-specific state
2. **Lift state up** - When multiple components need same state
3. **Context for global state** - Auth, theme, user preferences
4. **Separate server state** - API data lives in custom hooks
5. **No state libraries initially** - Hooks + Context handles most apps
6. **Avoid prop drilling** - Use context or composition instead

### Custom Hooks Pattern

Extract logic from components into reusable hooks:

- **Data fetching**: `useUser(id)`, `usePosts()`, `useProfile()`
- **Form state**: `useForm()`, `useInput()`
- **UI state**: `useModal()`, `useToggle()`, `useDebounce()`

**Pattern**: Return `{ data, loading, error, refetch }` for consistency

### Context Usage Guidelines

**When to use Context:**

- Authentication state and methods
- Theme/dark mode
- User preferences
- Feature flags

**Best practices:**

- Split contexts by concern (don't create one giant context)
- Always provide custom hook: `useAuth()`, `useTheme()`
- Throw error if used outside provider
- Keep context value stable to avoid re-renders

## Anti-Patterns to Avoid

### Component Anti-Patterns

- ❌ **Prop drilling** - Passing props through 3+ levels
- ❌ **Massive components** - 500+ line components
- ❌ **Default exports** - Harder to refactor
- ❌ **Class components** - Outdated pattern
- ❌ **Index as key** - Breaks React reconciliation
- ❌ **Mixing UI and logic** - Separate concerns

### State Anti-Patterns

- ❌ **Mutating state directly** - Always create new objects/arrays
- ❌ **useEffect for derived state** - Calculate directly instead
- ❌ **Too many useEffects** - Indicates poor design
- ❌ **Global state for everything** - Prefer local state
- ❌ **Premature state libraries** - Try Context first

### TypeScript Anti-Patterns

- ❌ **Using `any`** - Defeats purpose of TypeScript
- ❌ **Inline type definitions** - Extract to interfaces
- ❌ **Not typing API responses** - Always type external data
- ❌ **Optional chaining everywhere** (`?.`) - Fix types instead

## TypeScript Standards

- **Strict mode enabled** in `tsconfig.json`
- **Type everything** - Props, state, functions, API responses
- **Interfaces for objects** - Use `interface`, not `type`
- **Utility types** - `Partial<T>`, `Pick<T, K>`, `Omit<T, K>`
- **Generic components** - When truly reusable
- **Type imports** - `import type { User }` for type-only imports

## Responsive Design (Mobile-First)

### Core Principles

1. **Mobile-first CSS** - Start with mobile, add breakpoints for larger screens
2. **Fluid layouts** - Use percentages, `fr`, `min/max` instead of fixed widths
3. **Responsive typography** - `clamp()`, `rem` units
4. **Touch-friendly targets** - Minimum 44px tap targets
5. **Test on real devices** - Not just browser resize

### Breakpoint Strategy

```css
/* Mobile first - no media query needed */
.container { width: 100%; }

/* Tablet */
@media (min-width: 768px) { }

/* Desktop */
@media (min-width: 1024px) { }

/* Large desktop */
@media (min-width: 1440px) { }
```

## Performance Guidelines

1. **Measure before optimizing** - Use React DevTools Profiler
2. **Lazy load routes** - `React.lazy()` and `Suspense`
3. **Code splitting** - Split by route or feature
4. **Memoize sparingly** - Only when profiling shows need
5. **Virtualize long lists** - Only for 100+ items
6. **Optimize images** - WebP, lazy loading, responsive images
7. **Stable keys** - Never use array index as key

## Testing Strategy

- **Vitest** for unit tests
- **React Testing Library** for component tests
- **Playwright** or **Cypress** for E2E tests

### Testing Principles

- Test user behavior, not implementation
- Test critical paths: auth, data rendering, forms
- Mock API calls consistently
- Avoid testing library internals
- Write tests that resist refactoring

## API Integration

### Client Abstraction Pattern

- Create API client in `lib/api.ts`
- All API calls go through client
- Custom hooks consume API client
- Components never call API directly

### Data Fetching Architecture

- Custom hooks for each resource
- Handle loading, error, and empty states
- Return refetch function for manual updates
- Consider caching strategy (simple Map, or library if needed)

### Error Handling Strategy

- Error boundaries at route level for graceful failures
- User-facing error messages via toast/notification
- Log errors to monitoring service
- Fallback UI for broken components

## Accessibility (A11y)

- **Semantic HTML** - Use correct elements (`<button>`, `<nav>`, etc.)
- **ARIA labels** - For screen readers when semantics insufficient
- **Keyboard navigation** - Tab order, focus management
- **Color contrast** - WCAG AA minimum (4.5:1)
- **Focus indicators** - Visible focus states
- **Test with screen readers** - VoiceOver, NVDA

## Security Best Practices

### Authentication & Authorization

- Never store sensitive tokens in localStorage (use httpOnly cookies)
- Implement proper CSRF protection
- Validate on backend, not just frontend
- Use secure authentication flow

### Input Validation

- Sanitize user input before rendering
- Prevent XSS with proper escaping
- Validate on both client and server
- Use Content Security Policy headers

### Environment Variables

- Prefix public vars: `VITE_*`
- Access via `import.meta.env.VITE_API_URL`
- Never commit `.env.local` or secrets
- Different `.env` files per environment

## Development Workflow

### Initial Setup

```bash
npm create vite@latest my-app -- --template react-ts
cd my-app
npm install
npm run dev
```

### Common Commands

```bash
npm run dev          # Start dev server
npm run build        # Production build
npm run preview      # Preview production build
npm run lint         # Lint code (configure ESLint)
npm run test         # Run tests (configure Vitest)
```

### Code Quality Tools

- **ESLint** - Lint JavaScript/TypeScript
- **Prettier** - Format code consistently
- **Husky** - Git hooks for pre-commit checks
- **lint-staged** - Run linters on staged files

## Recommended Libraries

**Minimize dependencies**. Only add when needed.

- **Routing**: React Router
- **Forms**: React Hook Form (complex forms only)
- **Data fetching**: Custom hooks or TanStack Query (if caching needed)
- **UI components**: Build custom first, evaluate libraries later
- **Styling**: CSS Modules, Tailwind, or styled-components (pick one)
- **Date handling**: date-fns (lighter than moment.js)

## Related Skills

- [engineering-standards](../engineering-standards/SKILL.md) - Foundational principles (simplicity, TDD, architecture)
- [backend-development](../backend-development/SKILL.md) - API layer that frontend consumes

## Frontend-Specific Mindset

In addition to the universal principles in [engineering-standards](../engineering-standards/SKILL.md#engineering-mindset):

1. **Accessibility from start** - Not a feature, it's a requirement
2. **Mobile-first always** - Most users are on mobile
3. **Progressive enhancement** - Core functionality works without JS
4. **Error states matter** - Loading, error, empty states are UX
5. **Component composition** - Build complex UIs from simple, focused pieces
6. **Semantic HTML first** - Use the right elements before reaching for ARIA
