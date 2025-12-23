# Why This Framework Exists

## The Problem

When using AI coding assistants, teams typically work in an ad-hoc way: ask a question, get code, maybe it's good, ship it. This leads to three issues:

**Inconsistent quality**: Results vary widely depending on how you phrase requests and which team member asks.

**No pattern reuse**: The AI doesn't automatically check if similar code already exists in your project, so you end up with duplicate solutions.

**No learning**: Each interaction starts fresh. Teams don't capture what works or build on previous successes.

## What This Framework Does

Prompt Notes provides structure for working with AI assistants:

**Search before creating**: Workflows that check existing code before proposing new solutions.

**Compare alternatives**: Methods to generate and score multiple approaches instead of accepting the first suggestion.

**Systematic process**: Step-by-step workflows with verification points.

**Capture knowledge**: Templates that evolve as your project grows.

It's not a magic solution—it's a systematic approach that works better than improvising each time.

## Core Principles

### Search Before Building

Before generating new code, search your codebase for existing solutions. Most problems have been solved somewhere already—in `common/`, `utils/`, `hooks/`, or feature directories.

This reduces duplication and maintains consistency across your project.

### Multiple Approaches

Generate 2-4 different solutions and compare them using explicit criteria:
- Reusability
- Maintainability
- Alignment with existing patterns
- Complexity

Score each approach to make trade-offs visible rather than going with intuition or the first idea.

### Follow Existing Patterns

When patterns exist in your codebase, follow them exactly. Don't create new patterns unless necessary.

This makes code predictable and reduces cognitive load for the team.

### Minimal New Code

Prefer extending or adapting existing code over creating new implementations. Less code means fewer bugs and less maintenance.

### Verification Steps

Include checks at each phase:
- After discovery: confirm patterns found or none exist
- After design: verify approaches scored and compared
- After planning: check all steps are actionable
- After building: confirm tests pass and conventions followed

This catches issues early instead of at code review.

## How Workflows Work

### Without Structure

```
Developer: "Create a user dashboard"
AI: [generates code]
Developer: [commits it]
```

Problems: No checking for existing code, no comparison of alternatives, no verification steps.

### With This Framework

```
Developer: "@plan Create a user dashboard"
```

The workflow:
1. Loads relevant guidelines (architecture, code style, UI, testing)
2. Searches for existing dashboard components, user data hooks, and layouts
3. Documents what already exists and can be reused
4. Generates 2-4 different implementation approaches
5. Scores each approach based on reusability, maintainability, and pattern alignment
6. Creates a detailed PLAN.md with steps, files to modify, and verification checklist
7. Waits for confirmation before building

Then `@build` follows the plan step-by-step with verification at each stage.

Benefits: Reuses existing code, makes trade-offs explicit, catches issues early.

## Adapting the Framework

The framework includes examples using React, TypeScript, MUI, and GraphQL. You adapt these to your stack:

- Replace `makeStyles` with your styling approach
- Replace `Apollo GraphQL` with your data fetching
- Replace `Jest + RTL` with your testing framework
- Update directory paths to match your project structure

The methodology (search patterns, compare approaches, verify steps) stays the same regardless of tech stack.

## When to Use This

This framework works well for:
- Production codebases with multiple contributors
- Projects with established patterns
- Teams wanting consistent AI output quality

It's probably overkill for:
- One-off scripts or experiments
- Solo projects where you pair program everything
- Learning a new language (too much overhead)

## What Success Looks Like

You'll know it's working when:
- AI-generated code matches your existing patterns
- You spend less time in code review fixing style issues
- New team members produce consistent quality faster
- Fewer bugs from AI-generated code

The framework itself should evolve as you discover what works for your project.
