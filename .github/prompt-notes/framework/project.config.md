# Project Config

This file defines **project-specific** values used by the prompt framework.

## 🔧 Setup Instructions

1. Copy this entire folder into your project repository
2. Update all values below to match your project structure
3. The framework will reference these variables throughout all guidelines and workflows

## Paths

Configure where the framework stores its files and references your project structure:

- **SRC_DIR**: `src` — Your main source code directory
- **PROMPTS_DIR**: `.github/prompts` — Where these prompt files live in your repo
- **PLAN_PATH**: `.github/PLAN.md` — Generated project plans
- **DIAGRAM_PATH**: `.github/DIAGRAM.md` — Generated architecture diagrams
- **PROPOSAL_PATH**: `.github/PROPOSAL.md` — Feature proposals
- **TESTIM_DIR**: `.github/testim` — E2E test definitions (if using Testim or similar)

## Codebase Layout

Define your project's directory structure for shared code and utilities:

- **COMMON_DIR**: `src/common` — Shared/common code directory
- **COMMON_COMPONENTS_DIR**: `src/common/components` — Reusable components
- **COMMON_PAGES_DIR**: `src/common/pages` — Shared page templates
- **HOOKS_DIR**: `src/hooks` — Custom React hooks (adjust for your framework)
- **UTILS_DIR**: `src/utils` — Utility functions and helpers

## Stack

Document your technology choices (examples shown use React/MUI/GraphQL):

- **FRONTEND**: `React + TypeScript` — Your frontend framework and language
- **UI**: `MUI (Material-UI)` — Your UI component library
- **DATA**: `GraphQL/Apollo` — Your data fetching approach (REST, GraphQL, tRPC, etc.)

> **Note:** The framework guidelines include examples based on React/TypeScript/MUI/GraphQL. Adapt the patterns to your specific stack.

## Tooling

Configure your build and quality commands:

- **TYPECHECK_CMD**: `yarn tsc --noEmit` — Type checking command
- **LINT_CMD**: `yarn lint --fix` — Linting command
- **TEST_CMD**: `yarn test` — Test execution command

> **Tip:** Use `npm`, `pnpm`, or other package managers as appropriate for your project.
