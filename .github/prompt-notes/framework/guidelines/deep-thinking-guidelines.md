# Deep Thinking Guidelines

## Purpose
Core principles for systematic problem-solving, pattern discovery, and optimal decision-making.

---

## Core Principles

1. **Search Before Build** - Discover existing patterns first
2. **Multiple Approaches** - Explore 2-4 alternatives, not just first idea
3. **Quantitative Scoring** - Formula-based evaluation, not gut-feel
4. **Pattern Alignment** - Follow existing conventions exactly
5. **Minimal New Code** - Extend > Adapt > Create

---

## Pattern Discovery (Critical First Step)

**Before proposing ANY solution:**

### Search Strategy
1. **Semantic search**: `${COMMON_DIR}/`, `utils/`, `hooks/`, feature areas
2. **Grep search**: Function names, component patterns, imports, GraphQL ops
3. **Pattern analysis**: How is this solved? What conventions? What utilities exist?

### Convention Check
- `${PROMPTS_DIR}/guidelines/code-style-guidelines.md` - Coding style
- `${PROMPTS_DIR}/guidelines/architecture-guidelines.md` - Component design, state
- `${PROMPTS_DIR}/guidelines/testing-guidelines.md` - Testing approach

### Document Template
```
## Discovered Patterns
**Similar Implementations**: `path/file.ts` - <how it solves problem>
**Existing Utilities**: `functionName` in `utils.ts` - <reusable piece>
**Conventions**: <structure, naming, imports, styling, state management>
```

---

## Approach Exploration

Generate **2-4 distinct alternatives** with different trade-offs.

### Template
```
### Approach {N}: {Name}
**Description**: <1-sentence summary>
**Implementation**: <key steps>
**Characteristics**: Complexity (Low/Med/High), Reusability (1-5), Maintainability (1-5), Performance (1-5), Pattern Alignment (1-5)
**Pros**: ✅ <advantages>
**Cons**: ❌ <disadvantages>
**Files**: Modified/Created paths
```

---

## Solution Scoring

### Formula
```
score = (reusability × 0.25) + (maintainability × 0.25) + 
        (pattern_alignment × 0.30) + (performance × 0.10) - (complexity × 0.10)
```

### Scoring Guide
- **Complexity**: Low=1, Med=3, High=5 (subtracted)
- **Reusability**: 1 (not reusable) to 5 (highly reusable)
- **Maintainability**: 1 (hard) to 5 (easy)
- **Performance**: 1 (slow) to 5 (fast)
- **Pattern Alignment**: 1 (breaks patterns) to 5 (perfect fit)

### Weighting Table
```
| Approach | Reuse | Maintain | Pattern | Perf | Complex | Score |
|----------|-------|----------|---------|------|---------|-------|
| A        | 4     | 3        | 5       | 4    | 2       | 3.45  |
```

---

## Decision Framework

Choose **highest-scoring approach** or justify deviation.

### Recommendation Template
```
## Recommendation: {Name}
**Why**: <aligns with score, project context, long-term benefits>
**Trade-offs**: <what we're sacrificing, why acceptable>
**Rejected**: {Approach X}: <reason>
```

### Quality Gates
- [ ] Pattern discovery complete (2+ similar OR confirmed novel)
- [ ] 2+ approaches explored
- [ ] Quantitative scoring done
- [ ] Best choice justified
- [ ] Conventions checked
- [ ] Execution plan actionable

---

## Implementation Priority

1. **Reuse Existing** - Use as-is
2. **Extend Existing** - Add props, enhance
3. **Adapt Pattern** - Follow similar implementation
4. **Create Reusable** - Build for reuse
5. **Create Specific** - Only if truly unique

---

## Code Location Strategy

### Components
- `${COMMON_COMPONENTS_DIR}/` or `common/pages/` - Reusable
- Feature folders - Feature-specific only

### Utilities
- `${UTILS_DIR}/` - General, array, functional utils
- Feature utils - Feature-specific only

### Hooks
- `${HOOKS_DIR}/` - Reusable
- Feature hooks - Feature-specific only

**Decision**: Reusable (or likely) → common/ | Feature-specific → feature folder

---

## Duplication Avoidance

### Red Flags
Copy-pasting • Similar logic in 2+ places • Similar function/component names • Repeated patterns

### Action
Extract to shared utility/component and parameterize differences.

```typescript
// ❌ Duplication
const formatPathA = (path: string) => path.toUpperCase()
const formatPathB = (url: string) => url.toUpperCase()

// ✅ Extracted
const formatText = (text: string) => text.toUpperCase()
```

---

## Anti-Patterns

❌ Jump to implementation  
❌ Skip pattern discovery  
❌ Single approach only  
❌ Gut-feel decisions  
❌ Ignore conventions  
❌ Create duplicates

---

## Success Metrics

- ✅ 70%+ code reuse (when applicable)
- ✅ 100% convention compliance
- ✅ 2+ genuine alternatives explored
- ✅ Quantitative scoring applied
- ✅ Clear decision rationale
- ✅ Minimal new code created
- ✅ Implementation matches plan

---

## Thinking Time

**Prevents**: Wrong approach → hours refactoring | Skip discovery → duplicate code | No alternatives → miss better solution
