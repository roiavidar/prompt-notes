# @deep-think Command

## Objective
Systematic workflow for complex requests: analyze → discover patterns → explore approaches → score → choose best → execute.

**Core Methodology**: `${PROMPTS_DIR}/guidelines/deep-thinking-guidelines.md`

---

## 7-Phase Workflow

1. **Analysis** - Understand: literal request, intent, core problem, constraints, success criteria
2. **Pattern Discovery** - Search codebase FIRST (`${COMMON_DIR}/`, `${UTILS_DIR}/`, `${HOOKS_DIR}/`) - see deep-thinking-guidelines.md
3. **Decomposition** - Break into 3-7 atomic steps with rationale
4. **Approach Exploration** - Generate 2-4 distinct alternatives - see deep-thinking-guidelines.md
5. **Solution Weighting** - Score using formula - see deep-thinking-guidelines.md
6. **Recommendation** - Select highest-scoring approach with justification - see deep-thinking-guidelines.md
7. **Execution** - Implement following discovered patterns

---

## Usage

```
@deep-think <user request>
@deep-think <file-path> - <request>
```

**Examples**:
```
@deep-think add loading spinner to data table
@deep-think src/Table.tsx - add column sorting
```

---

## When to Use

**Use for**: Complex/ambiguous requests • Multiple valid approaches • Pattern discovery critical • Trade-offs need consideration

**Skip for**: Simple requests • Pattern already clear • Urgent quick fixes

---

## Output Format

```markdown
# 🧠 Deep Analysis: {Title}

## 1. Request Analysis
**Literal**: <what user said>
**Intent**: <what user needs>
**Core Problem**: <fundamental issue>
**Constraints**: <technical/architectural>
**Success Criteria**: <what "good" looks like>

## 2. Discovered Patterns
<Use template from deep-thinking-guidelines.md>

## 3. Step Breakdown
1. <Step> - <why>
2. <Step> - <why>

## 4. Approach Exploration
<Use template from deep-thinking-guidelines.md>

## 5. Solution Weighting
<Use scoring table from deep-thinking-guidelines.md>

## 6. Recommendation
<Use recommendation template from deep-thinking-guidelines.md>

## 7. Execution Plan
- [ ] Task items
```

Then implement.

---

## Execution Checklist

- [ ] **Imports**: React first, named before default, sorted by length
- [ ] **Patterns**: Match existing hooks/state/styling
- [ ] **Location**: common/ (reusable) vs feature-specific
- [ ] **Reuse**: Existing utilities/hooks/components
- [ ] **Naming**: PascalCase/camelCase per convention
- [ ] **Types**: Explicit annotations
- [ ] **Comments**: Only if pattern has them
- [ ] **Tests**: If critical path

---

## Key Principles

🧠 Think deeply BEFORE acting  
🔍 Discover patterns FIRST  
⚖️ Weigh systematically  
✅ Choose BEST not first  
🎯 Execute precisely

**Goal**: Better decisions through structured thinking (~90s analysis prevents hours of refactoring).

---