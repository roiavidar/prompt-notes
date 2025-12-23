# Prompt Notes

A framework for structuring AI assistant prompts in software projects.

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
cp -r prompt-notes/framework/ your-project/.ai-prompts/
cd your-project/.ai-prompts/
```

### 2. Configure Paths and Stack

Edit `project.config.md` with your project's details:

```markdown
## Paths
- SRC_DIR: src
- PROMPTS_DIR: .ai-prompts
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

See [docs/CUSTOMIZATION.md](docs/CUSTOMIZATION.md) for guidance on translating patterns to your stack.

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
│   └── testim-instructions.md           # E2E tests (optional)
├── utilities/              # Meta-tools
│   ├── deep-think-instructions.md       # Deep analysis
│   ├── summarize-instructions.md        # Summarization
│   ├── generate-spec-instructions.md    # Command → Spec
│   └── generate-command-instructions.md # Spec → Command
└── commands/
    └── commands.md         # Quick reference
```

## How It Works

### Pattern Discovery

Before generating code, the AI searches your existing codebase for similar solutions in `common/`, `utils/`, and `hooks/` directories. This reduces duplication and maintains consistency.

### Multiple Approaches

The framework encourages generating 2-4 different solutions and scoring them based on factors like reusability, maintainability, and pattern alignment. This makes trade-offs explicit rather than accepting the first suggestion.

### Structured Phases

Workflows follow a consistent structure:
1. Discovery - Find existing patterns
2. Design - Explore alternatives
3. Plan - Create detailed steps
4. Build - Implement with checks
5. Test - Cover critical functionality
6. Verify - Confirm it works

### Quality Checks

Each phase includes verification steps to catch issues early. For example, after planning, you check that all steps are actionable and all files are listed.

### Self-Improvement

The framework includes tools to create new workflows and commands as you discover patterns specific to your project.

## Example: Planning a Feature

```bash
@plan Add real-time notifications to dashboard
```

This workflow:
1. Loads relevant guideline files
2. Searches for existing notification or real-time patterns
3. Generates 2-4 implementation approaches
4. Scores each approach
5. Creates a detailed PLAN.md file with steps and checklists

## Stack Support

The framework works with any stack but requires customization:

**Languages**: JavaScript, TypeScript, Python, Go, Java, etc.  
**Frameworks**: React, Vue, Angular, Django, Rails, etc.  
**UI Libraries**: MUI, Tailwind, Vuetify, Bootstrap, etc.  
**Data Layers**: GraphQL, REST, tRPC, etc.  
**Testing**: Jest, Vitest, Pytest, JUnit, etc.

You'll need to adapt import patterns, styling approaches, testing setup, and error handling to match your project.

## Documentation

- [PHILOSOPHY.md](PHILOSOPHY.md) - Background and approach
- [docs/CUSTOMIZATION.md](docs/CUSTOMIZATION.md) - Adapting to your stack
- [docs/VARIABLES.md](docs/VARIABLES.md) - Configuration reference

## License

MIT

---

This framework provides structure for working with AI assistants. The value is in applying the methodology to your specific project, not in using the examples as-is.
