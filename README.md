# Prompt Notes 🎼

Reusable, structured prompt workflows and guidelines for AI coding assistants.

## What This Is

This is a collection of prompt templates and workflows for working with AI coding assistants. It's not a set of universal prompts—it's a system you adapt to your project's specific needs.

The framework includes guidelines for code structure, workflows for common tasks (planning, building, testing), and tools for creating new prompts as needed.

## Why Use This

When working with AI assistants, most teams don't have a consistent approach. This leads to:
- Inconsistent code quality
- Reinventing solutions that already exist in the codebase
- Accepting the first AI suggestion without exploring alternatives
- No way to capture and share what works

This framework addresses these issues by providing:
- A systematic approach to problem-solving
- Workflows that check existing code before creating new solutions
- Methods for comparing multiple approaches
- Templates that can evolve with your project

## Getting Started

### 1. Copy the Framework

```bash
cp -r prompt-notes/framework/ your-project/.github/prompt-notes/
cd your-project/.github/prompt-notes/
```

### 2. Configure Paths and Stack

Edit `project.config.md` with your project's details:

```markdown
## Paths
- SRC_DIR: src
- PROMPTS_DIR: .github/prompt-notes
- COMMON_DIR: src/shared

## Stack
- FRONTEND: Vue + TypeScript
- UI: Vuetify
- DATA: REST + Axios

## Tooling
- TYPECHECK_CMD: npm run typecheck
- LINT_CMD: npm run lint
- TEST_CMD: npm run test
```

### 3. Adapt to Your Stack

The framework includes examples using React/TypeScript/MUI/GraphQL. Each file notes what needs adaptation:

```markdown
> Stack Context: These use MUI components. Adapt to your UI library.
```

See [CUSTOMIZATION.md](.github/prompt-notes/docs/CUSTOMIZATION.md) for guidance on translating patterns to your stack.

### 4. Use the Workflows

```bash
@plan Create user dashboard
@build
@test ComponentName.tsx
```

## Framework Structure

```
framework/
├── project.config.md       # Configure paths, stack, commands
├── guidelines/             # Core principles & best practices
│   ├── deep-thinking-guidelines.md      # Pattern discovery, scoring
│   ├── architecture-guidelines.md       # Component design, state
│   ├── code-style-guidelines.md         # Conventions & imports
│   ├── ui-guidelines.md                 # Design implementation
│   ├── testing-guidelines.md            # What/when to test
│   └── error-handling-guidelines.md     # Error patterns
├── workflows/              # End-to-end processes
│   ├── plan-instructions.md             # Feature planning
│   ├── build-instructions.md            # Implementation
│   ├── test-instructions.md             # Test generation
│   ├── diagram-instructions.md          # Architecture docs
│   ├── ui-accurate-instructions.md      # Pixel-perfect UI
│   ├── stress-ui-instructions.md        # Comprehensive testing
│   ├── initiative-instructions.md       # Find improvements
│   ├── extract-concerns-instructions.md # Refactoring
│   └── e2e-instructions.md              # E2E test generation (example)
├── utilities/              # Meta-tools
│   ├── deep-think-instructions.md       # Deep analysis
│   ├── summarize-instructions.md        # Summarization
│   ├── generate-spec-instructions.md    # Extract reusable template from instruction
│   └── generate-command-instructions.md # Create project-specific instruction from template
└── commands/
    └── commands.md         # Quick reference
```

## How It Works

Example workflow:

```bash
@plan Add real-time notifications to dashboard
```

The framework:
1. Searches your codebase for existing notification/real-time patterns (`common/`, `utils/`, `hooks/`)
2. Generates 2-4 implementation approaches
3. Scores each based on reusability, maintainability, and pattern alignment
4. Creates a detailed PLAN.md with actionable steps and verification checklist
5. Waits for your confirmation before building

Then `@build` implements the plan with checks at each step, and `@test` generates tests for critical functionality.

## Creating Custom Workflows

As you discover patterns specific to your project, you can create new instructions:

1. **Extract a template**: `@generate-spec build-instructions.md` creates a reusable spec
2. **Reuse elsewhere**: Copy the spec to another project
3. **Generate instruction**: `@generate-command build-instructions-spec.md` fills in project details

This lets you capture and share workflows that work for your team.

## Stack Support

The framework works with any stack but requires customization:

You'll need to adapt import patterns, styling approaches, testing setup, and error handling to match your project.

## Documentation

- [PHILOSOPHY.md](.github/prompt-notes/docs/PHILOSOPHY.md) - Background and approach
- [ARCHITECTURE.md](.github/prompt-notes/docs/ARCHITECTURE.md) - Visual framework overview
- [CUSTOMIZATION.md](.github/prompt-notes/docs/CUSTOMIZATION.md) - Adapting to your stack
- [VARIABLES.md](.github/prompt-notes/docs/VARIABLES.md) - Configuration reference

## Background

This repository is based on ideas described in the following article:
[From Chatting with AI to Architecting It](https://dev.to/roi_d95df0bf6e689c571b39a/from-chatting-with-ai-to-architecting-it-5a9k)

## License

MIT

---

This framework provides structure for working with AI assistants. The value is in applying the methodology to your specific project, not in using the examples as-is.
