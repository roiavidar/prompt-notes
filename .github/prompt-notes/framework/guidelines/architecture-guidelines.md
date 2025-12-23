# Architecture Guidelines

> **Stack Context:** These guidelines are based on React/TypeScript. Adapt patterns to your framework as needed.

React/TypeScript architecture principles.

## Component Design
**Composition over inheritance**: Build small, focused components  
**Single responsibility**: One purpose per component  
**Controlled components**: Forms use value + onChange  
**Structure**: Imports → Types → Component → Styles (makeStyles)

## State Management
**Local State (useState)**: UI-only (modals, toggles, form inputs)  
**Server State (Apollo)**: GraphQL data (auto-cached)  
**Derived State**: Calculate on render, don't store
**Prop Drilling**: 0-2 levels direct, 3+ use hooks/context

## Data Flow
**GraphQL + Apollo**: Single source of truth (cache)  
**Cache Strategy**:
- Optimistic updates for instant UX
- Cache eviction (preferred) over refetch
- Error handling with user-friendly messages

## Module Organization
**Structure**: Feature-first (e.g., `feature-a/`, `feature-b/`) with shared `common/`

**Move to `common/` when**:
- Used by 2+ features
- Has clear, generic purpose
- No feature-specific logic

**Utils**: Pure functions only. Location by purpose (utils.ts, arrayUtils.ts, functionalUtils.ts)

## Performance
**Memoization**: Use sparingly, only when profiling shows benefit  
**Callbacks**: `useCallback` to prevent child re-renders  
**Code Splitting**: Dynamic imports for large features  
**Philosophy**: Clear code first, profile before optimizing

## Design Patterns
**Custom Hooks**: Extract when used 2+ times or complex state  
**Component Logic Separation**: If a component has too much logic (5+ useState, complex handlers, orchestration), extract to a custom hook  
**Hook Reusability Check**: Before creating a new hook, check if existing hooks can be generalized or if the logic is a common concern that should be consolidated  
**Render Props**: For flexible composition  
**HOCs**: Prefer hooks (more composable)  
**Container/Presenter**: Separate logic from UI

## TypeScript
**Type Safety**: Interfaces for objects, strict settings, avoid `any`, use codegen  
**Prop Types**: Clear interfaces with explicit types, avoid `[key: string]: any`  

## Testing
**Key**: Test critical functionality, not implementation details.  
**Avoid Duplication**: Check for duplicate test cases. If found in the same file, merge them into a single comprehensive test.

## Code Reusability
Whenever possible, try to reuse existing components or functions instead of creating new ones. If you find yourself writing similar code in multiple places, consider extracting it into a reusable component/hook. Check for existing implementations in `${UTILS_DIR}/` folder for relevant utils functions and `${COMMON_DIR}/` folder for common components.
