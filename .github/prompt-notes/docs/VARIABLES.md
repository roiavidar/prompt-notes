# Variables Reference

Configuration variables used in the framework.

## Overview

Variables like `${VARIABLE_NAME}` appear throughout the framework. Define them once in `project.config.md` and they're used everywhere.

This makes the framework portable—update paths in one place instead of editing every file.

## Configuration

Edit `framework/project.config.md`:

```markdown
## Paths
- SRC_DIR: src
- PROMPTS_DIR: .ai-prompts
- COMMON_DIR: src/shared
- UTILS_DIR: src/utils
- HOOKS_DIR: src/hooks

## Stack
- FRONTEND: React + TypeScript
- UI: MUI
- DATA: GraphQL/Apollo

## Tooling
- TYPECHECK_CMD: npm run typecheck
- LINT_CMD: npm run lint
- TEST_CMD: npm test
```

## Variable List

### Path Variables

**SRC_DIR** - Your source code directory  
Default: `src`  
Example: `${SRC_DIR}/components/UserProfile.tsx`

**PROMPTS_DIR** - Where this framework lives  
Default: `.github/prompts`  
Example: `${PROMPTS_DIR}/guidelines/architecture.md`

**COMMON_DIR** - Shared code  
Default: `src/common`  
Used for: Pattern discovery

**UTILS_DIR** - Utility functions  
Default: `src/utils`  
Used for: Finding helper functions

**HOOKS_DIR** - Custom hooks/composables  
Default: `src/hooks`  
Used for: Finding reusable logic

**COMMON_COMPONENTS_DIR** - Shared UI components  
Default: `src/common/components`  
Used for: UI pattern discovery

**PLAN_PATH** - Where @plan writes output  
Default: `.github/PLAN.md`

**DIAGRAM_PATH** - Where @diagram writes output  
Default: `.github/DIAGRAM.md`

**PROPOSAL_PATH** - Where @initiative writes proposals  
Default: `.github/PROPOSAL.md`

### Stack Variables

**FRONTEND** - Your framework and language  
Example: `Vue 3 + TypeScript`, `React + JavaScript`, `Angular 17`

**UI** - Your UI library  
Example: `MUI`, `Tailwind CSS`, `Vuetify`, `Bootstrap`

**DATA** - Your data layer  
Example: `GraphQL/Apollo`, `REST + Axios`, `tRPC`, `TanStack Query`

### Tooling Variables

**TYPECHECK_CMD** - Command to run type checking  
Example: `npm run typecheck`, `yarn tsc --noEmit`

**LINT_CMD** - Command to run linter  
Example: `npm run lint`, `eslint . --fix`

**TEST_CMD** - Command to run tests  
Example: `npm test`, `vitest run`, `pytest`

## Usage

In guidelines and workflows, reference variables instead of hard-coding paths:

**Instead of:**
```markdown
Check src/common/components/ for existing components
```

**Use:**
```markdown
Check ${COMMON_COMPONENTS_DIR}/ for existing components
```

When you change your directory structure, update `project.config.md` once instead of editing multiple files.

## Custom Variables

Add your own as needed:

```markdown
## Custom
- PROJECT_MODULE: @/app
- API_BASE: src/api
- STYLES_DIR: src/styles
```

Then use them: `${PROJECT_MODULE}/theme`, `${API_BASE}/client`
**Purpose**: Shared page templates  
**Default**: `src/common/pages`  
**Used in**: Architecture guidelines  
