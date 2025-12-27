# Framework Architecture

Visual overview of the Prompt Notes framework structure and component relationships.

## Summary

The Prompt Notes framework is a structured system for AI-assisted development workflows. It consists of configuration files, reusable guidelines, workflow instructions, and utility tools organized in a portable directory structure designed to be copied into any project.

## Key Patterns
- **Variable-based paths** - All paths use `${VARIABLE}` placeholders defined in project.config.md
- **Separation of concerns** - Guidelines (principles) vs Instructions (workflows) vs Utilities (meta-tools)
- **Stack-agnostic methodology** - Core patterns remain constant regardless of tech stack
- **Self-documenting** - Each file includes inline examples and adaptation notes

## Data Flow

Configuration starts in project.config.md defining all paths and stack details. Guidelines provide the principles (deep-thinking, architecture, code-style, ui, testing, error-handling). Workflow instructions reference these guidelines and use variable placeholders to remain portable. Utility tools enable meta-operations like creating new commands or analyzing existing ones. The commands.md file serves as the entry point mapping command names to instruction files.

## Component Relationships

```mermaid
flowchart TB
    CONFIG[Configuration<br/>project.config.md<br/>commands.md]
    
    subgraph GUIDELINES[Guidelines - Core Principles]
        G1[Deep Thinking]
        G2[Architecture]
        G3[Code Style]
        G4[UI Guidelines]
        G5[Testing]
        G6[Error Handling]
    end
    
    subgraph WORKFLOWS[Workflows - Processes]
        W1[diagram - Documentation]
        W2[plan - Planning]
        W3[build - Implementation]
        W4[test - Testing]
        W5[ui-accurate - UI Refinement]
        W6[stress-ui - Mock Data]
        W7[extract-concerns - Refactoring]
        W8[initiative - Improvements]
        W9[e2e - E2E Tests]
    end
    
    subgraph UTILS[Utilities - Meta Tools]
        U1[deep-think - Analysis]
        U2[summarize - Summarization]
        U3[generate-spec - Templates]
        U4[generate-command - Commands]
    end
    
    CONFIG --> GUIDELINES
    CONFIG --> WORKFLOWS
    CONFIG --> UTILS
    
    GUIDELINES --> W2
    GUIDELINES --> W3
    GUIDELINES --> W4
    GUIDELINES --> W7
    GUIDELINES --> W8
    
    G1 --> W1
    G1 --> U1
    
    W2 -->|creates| OUT1[PLAN.md]
    W1 -->|creates| OUT2[DIAGRAM.md]
    W8 -->|creates| OUT3[PROPOSAL.md]
    W9 -->|creates| OUT4[e2e/*.json]
    
    style CONFIG fill:#e1f5ff
    style G1 fill:#fff4e1
    style U1 fill:#e8f5e9
    style U3 fill:#e8f5e9
    style U4 fill:#e8f5e9
```

## Directory Structure

```mermaid
flowchart TB
    ROOT[.github/prompt-notes/]
    
    ROOT --> DOCS[docs/]
    ROOT --> FW[framework/]
    
    DOCS --> D1[PHILOSOPHY.md]
    DOCS --> D2[CUSTOMIZATION.md]
    DOCS --> D3[VARIABLES.md]
    DOCS --> D4[ARCHITECTURE.md]
    
    FW --> F1[project.config.md]
    FW --> F2[commands/]
    FW --> F3[guidelines/]
    FW --> F4[instructions/]
    FW --> F5[util/]
    
    F3 --> G[6 guideline files]
    F4 --> W[9 workflow files]
    F5 --> U[4 utility files]
    
    style ROOT fill:#e1f5ff
    style FW fill:#fff4e1
    style F3 fill:#e8f5e9
    style F4 fill:#ffe8e8
    style F5 fill:#f3e8ff
```

## File Organization

### Configuration
- `project.config.md` - Central configuration for paths, stack, and tooling
- `commands/commands.md` - Command registry mapping @commands to files

### Guidelines (6 files)
Core principles referenced by multiple workflows:
- Deep thinking methodology
- Architecture patterns
- Code style conventions
- UI guidelines
- Testing approach
- Error handling patterns

### Workflows (9 files)
End-to-end processes that produce deliverables:
- `@plan` - Feature planning
- `@build` - Implementation
- `@test` - Test generation
- `@diagram` - Architecture docs
- `@initiative` - Improvement proposals
- `@extract-concerns` - Duplication analysis
- `@ui-accurate` - UI refinement
- `@stress-ui` - Mock data
- `@e2e` - E2E tests

### Utilities (4 files)
Meta-tools for framework operations:
- `@deep-think` - Deep analysis
- `@summarize` - File summarization
- `@generate-spec` - Create templates
- `@generate-command` - Generate commands

### Documentation (4 files)
- PHILOSOPHY.md - Why and when
- CUSTOMIZATION.md - How to adapt
- VARIABLES.md - Configuration reference
- ARCHITECTURE.md - Visual framework overview

## Design Principles

**Portability**: The framework uses `${VARIABLE}` placeholders throughout all files, making it portable across different project structures. Users only need to update `project.config.md` once.

**Stack Agnostic**: While examples use React/TypeScript/MUI/GraphQL, the methodology (search patterns, compare approaches, verify steps) applies to any tech stack. Users adapt the syntax but keep the process.

**Self-Improving**: The framework includes meta-tools (`@generate-spec`, `@generate-command`) that allow users to create new workflows and commands as they discover patterns specific to their projects.

**Guidelines vs Instructions**: Guidelines define principles and are referenced by multiple workflows. Instructions are executable workflows that load relevant guidelines and produce concrete outputs (PLAN.md, DIAGRAM.md, etc.).

**Verification-Focused**: Most workflows include verification checklists and quality gates to catch issues early rather than at code review stage.
