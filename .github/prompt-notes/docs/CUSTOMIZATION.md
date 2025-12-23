# Customization Guide

How to adapt this framework to your tech stack.

## Quick Start

1. **Copy the framework**
   ```bash
   cp -r prompt-notes/framework/ your-project/.ai-prompts/
   ```

2. **Update `project.config.md`** with your paths and stack:
   ```markdown
   ## Paths
   - SRC_DIR: src
   - PROMPTS_DIR: .ai-prompts
   - COMMON_DIR: src/shared
   
   ## Stack
   - FRONTEND: Vue 3 + TypeScript
   - UI: Vuetify 3
   - DATA: REST + Axios
   
   ## Tooling
   - TYPECHECK_CMD: npm run typecheck
   - LINT_CMD: npm run lint
   - TEST_CMD: npm test
   ```

3. **Adapt guidelines** - Each file has inline examples. Update them for your stack while keeping the methodology.

4. **Test it** - Use `@plan` on a small task and check if the output matches your conventions.

## Translation Examples

The framework uses React/TypeScript/MUI/GraphQL examples. Here's how to translate common patterns:

### React → Vue

**Component structure:**
- `useState` → `ref`/`reactive`
- `useEffect` → `watch`/`onMounted`
- JSX → `<template>` syntax

**Keep the same:** Component organization principles, state management approach.

### GraphQL → REST

**Data fetching:**
- `useQuery(GET_USER)` → `fetch('/api/users')`
- Apollo error handling → HTTP status codes

**Keep the same:** Error handling philosophy, retry logic, loading states.

### MUI → Tailwind

**Styling:**
- `makeStyles()` → Tailwind classes
- `theme.palette.primary` → `bg-blue-600`
- `padding: "1.5rem"` → `p-6`

**Keep the same:** Spacing scale (rem-based), responsive design approach.

### Jest → Vitest

**Testing:**
- `import { render } from '@testing-library/react'` → your test framework
- `customRender` helper → your equivalent

**Keep the same:** Test critical functionality only, avoid implementation details.

## What to Change vs Keep

**Change:**
- Import statements and paths
- Component/function syntax
- Library-specific APIs
- Tooling commands

**Keep:**
- Search before building
- Generate multiple approaches
- Score alternatives
- Verify at each step
- Reuse over create

## Common Customizations

**Paths:** Update `SRC_DIR`, `COMMON_DIR`, `UTILS_DIR`, `HOOKS_DIR` to match your structure.

**Stack descriptions:** Update to accurately describe your tech (helps AI context).

**Commands:** Replace `TYPECHECK_CMD`, `LINT_CMD`, `TEST_CMD` with your actual commands.

**Styling patterns:** Replace MUI examples with your UI library in `ui-guidelines.md`.

**Import rules:** Update import examples in `code-style-guidelines.md` for your conventions.

## Start Small

Don't customize everything at once:

1. Start with `code-style-guidelines.md` (most project-specific)
2. Try a workflow like `@plan`
3. Check if output matches your patterns
4. Gradually customize more files as needed

The goal is having the AI follow your project's existing conventions, not creating new ones.

#### Test Structure

**Jest + RTL (example):**
```ts
import { customRender, screen } from 'src/test-utils'

const { user } = customRender(<UserProfile />, {
