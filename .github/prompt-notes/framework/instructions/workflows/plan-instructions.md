## Planning & Implementing Workflow

**CRITICAL**: When user requests start with `@plan`, follow this systematic approach before any implementation.

**STEP 0: Load ALL Guidelines** (DO THIS FIRST):

Read all five guideline files before starting discovery:
1. `${PROMPTS_DIR}/guidelines/deep-thinking-guidelines.md` - Pattern discovery, approach exploration, scoring
2. `${PROMPTS_DIR}/guidelines/architecture-guidelines.md` - Component design, state management, data flow patterns
3. `${PROMPTS_DIR}/guidelines/ui-guidelines.md` - Design fidelity, spacing, responsive design, component selection
4. `${PROMPTS_DIR}/guidelines/testing-guidelines.md` - When and how to write tests, integration patterns
5. `${PROMPTS_DIR}/guidelines/error-handling-guidelines.md` - GraphQL errors, REST errors, retry logic, user feedback

**Guidelines must be loaded in parallel** to inform discovery phase.

**STEP 0.5: Screenshot Analysis** (If Figma screenshots provided):

When user provides screenshots with `@plan`, perform UI analysis BEFORE discovery:

1. **Extract Visual Specifications** - Create JSON spec (see ui-accurate-instructions.md format):
   - Measure spacing in pixels from screenshot
   - Convert to rem (px / 16)
   - Round to nearest theme spacing: 0.25, 0.5, 0.75, 1, 1.25, 1.5, 2, 2.5, 3, 3.5
   - Identify layout structure (Grid/Stack/Box)
   - Document component types, sizes, alignments
   - Note padding, gaps, margins

2. **Component Mapping** - Match screenshot elements to MUI/custom components:
   - Check `${COMMON_COMPONENTS_DIR}/` for existing components
   - Identify reusable patterns (cards, tables, forms, dialogs)
   - Note Typography variants needed (h1-h6, body1/body2, caption)

3. **Add to PLAN.md** - Include UI specifications section:
   ```markdown
   ## UI Specifications (from screenshots)
   
   **Spacing Scale**: 0.5rem, 1rem, 1.5rem, 2rem, 3rem
   
   **Layout Structure**:
   - Container: Grid, padding: 1rem 3rem 0 3rem
   - Header: Stack direction=row, spacing=2, justifyContent=space-between
   - Content: Stack direction=column, spacing=1.5
   
   **Components**:
   - Title: Typography variant=h5, fontWeight=600, marginBottom=1rem
   - Cards: Paper, padding=1.5rem, gap=1rem between
   - Buttons: variant=outlined, size=medium, spacing=0.5rem
   
   **Responsive**:
   - xs: Stack direction=column
   - md: Grid with 2 columns
   - lg: Grid with 3 columns
   ```

4. **Apply ui-guidelines.md** - Ensure plan follows:
   - Rem-based spacing (no hardcoded pixels)
   - Proper component selection (Grid2 vs Stack vs Box)
   - Typography variants always specified
   - Theme colors (palette.grey, palette.primary, etc.)
   - Responsive breakpoints (xs/sm/md/lg/xl)
   - Minimal wrappers strategy
   - **CRITICAL**: Single CSS property → use `style` prop or MUI prop (e.g., `padding="1rem"`)
   - **CRITICAL**: Multiple CSS properties (2+) → use `makeStyles` class (NOT `sx`, NOT inline `style`)

---

### 1. Discovery Phase (Search Before You Build)

**Reference**: See `${PROMPTS_DIR}/guidelines/deep-thinking-guidelines.md` for complete pattern discovery strategy.

**Search for existing implementations** using `semantic_search` and `grep_search`:
- `${COMMON_DIR}/` - Reusable components
- `${UTILS_DIR}/` - Utility functions
- `${HOOKS_DIR}/` - Custom hooks
- `src/routing/` - Routing patterns

**Analyze existing patterns** - Identify how similar problems are solved

**Evaluate 2-3 approaches** - Apply scoring methodology from deep-thinking-guidelines.md

---

### 2. Combine Screenshot + Discovery (If Screenshots Provided)

**When screenshots included**: Combine UI specs from STEP 0.5 with patterns from discovery

**Strategy**: Build [UI requirements] using [discovered patterns]

---

### 3. PLAN.md Creation

**Delete existing PLAN.md** before creating new one:
```bash
rm ${PLAN_PATH}
```

**Create new** at `${PLAN_PATH}` using template (section 6 below)

**CRITICAL**: 
- Always REPLACE entire file - never merge or append sections
- Include UI specs from screenshots + reusable patterns from discovery
- **Wait for user approval** - DO NOT implement until approved

**Prompt user to proceed**:
```
✅ PLAN.md created successfully

Ready to implement? Run: @build
```

---

### 4. Implementation Priority

**Reference**: See `${PROMPTS_DIR}/guidelines/deep-thinking-guidelines.md` for complete priority framework.

1. **Reuse existing code** - Extend or compose existing components/utilities
2. **Adapt similar patterns** - Follow established patterns
3. **Create new reusable code** - Build to be reusable from day one
4. **Create specific implementation** - Only if truly unique

### 5. Code Location

**Components**:
- `${COMMON_COMPONENTS_DIR}/` or `common/pages/` - Reusable across features
- Feature folders - Feature-specific only

**Utils**:
- `${UTILS_DIR}/` (utils.ts, arrayUtils.ts, or functionalUtils.ts) - Generalizable
- Feature utils - Feature-specific only

**Hooks**:
- `${HOOKS_DIR}/` - Reusable
- Feature hooks - Feature-specific only

### 6. Avoid Code Duplication

**Red flags**: Copy-pasting, similar logic in 2+ places, similar function names
**Action**: Extract to shared utility/component and parameterize differences

**When duplication is significant**: Use `@extract-concerns` command to:
- Analyze changed files for common patterns
- Search project for existing similar code
- Propose shared components/hooks/utilities
- Reduce diff size by extracting abstractions

**Example**: `@extract-concerns` (analyze all changed files) or `@extract-concerns ${SRC_DIR}/pages/feature-a/DataTable.tsx`

### 7. PLAN.md Template

**CRITICAL**: Keep plans CONCISE to avoid missing important details. Focus on WHAT and WHY, not verbose HOW.

Use this template when creating `${PLAN_PATH}`:

```markdown
# Feature: [Feature Name]

## Discovery
- **Existing patterns**: [Similar implementations/files to reference]
- **Reusable code**: [What exists vs. needs creation]

## UI Specifications (if screenshots provided)
- **Spacing**: [Key measurements in rem]
- **Layout**: [Grid/Stack structure, padding, gaps]
- **Components**: [Typography variants, button styles, etc.]
- **Responsive**: [Breakpoint behavior]

## Strategy
**Approach**: [Chosen strategy - 1-2 sentences]

**Why**: [Key reason(s)]

**Alternatives**: [Other options considered - brief]

## Changes
**New files**:
- `path/file.ext` - Purpose

**Modified files**:
- `path/file.ext` - Changes summary

**Dependencies**: [If any]

## Implementation Steps
1. [Critical step 1]
2. [Critical step 2]
3. [Critical step 3]

**After implementation**: Consider running `@extract-concerns` to identify and eliminate duplication

## Risks
- [Breaking change 1]
- [Area to test carefully]

## Testing
**Critical tests only**:
- [ ] [Test category 1 - what needs coverage]
- [ ] [Test category 2 - what needs coverage]

## QA Checklist
- [ ] All states render correctly (loading, error, success, empty)
- [ ] Interactive elements functional
- [ ] Form validation works
- [ ] No console errors
- [ ] Keyboard navigation works
- [ ] TypeScript/lint passes

## Done
- [ ] Code follows patterns, no duplication
- [ ] Error handling implemented
- [ ] Manual testing complete
- [ ] Locale strings added (if user-facing)
```

**Guidelines for concise plans**:
- **Avoid**: Verbose explanations, detailed code snippets, extensive examples, repetitive sections
- **Include**: Essential info only - patterns, file changes, critical steps, risks
- **Length**: Aim for 500 lines max (not 1000+)

### 8. During Implementation

**Progress**: Mark completed items with [x], document blockers in PLAN.md

**Breaking existing features** - CRITICAL:
- Use `list_code_usages` and `grep_search` before removing/changing code
- Update ALL usages when modifying shared functions/components
- Run `${TYPECHECK_CMD}` to verify no TypeScript errors

**Cleanup**: Remove unused imports, debug console.logs, addressed TODOs

**Deviations**:
- Minor (implementation details): Document in comments
- Major (architecture): Update PLAN.md and notify user

### 9. When to Ask for Clarification

Ask when:
- Multiple valid approaches with significant trade-offs exist
- Requirements conflict with existing patterns
- Decision impacts multiple features
- Breaking changes needed

**Example**: "I found three approaches: 1) [option] 2) [option] 3) [option]. Approach 1 seems best because [reasoning]. Does this align with your expectations?"

### 10. Completion Checklist

**Before marking complete**:
- [ ] All Definition of Done items checked [x]
- [ ] Feature works as expected, no breaking changes
- [ ] TypeScript/lint passes (`${TYPECHECK_CMD}`, `yarn lint`)
- [ ] No console errors or warnings
- [ ] Locale strings added (if user-facing):
  - [ ] `${LOCALE_DIR}/type.ts` - Type definition
  - [ ] `${LOCALE_DIR}/en.ts` - English text

**If implementation fails**:
1. Add "Post-Mortem" section to PLAN.md explaining why
2. Notify user with lessons learned
3. Suggest alternative approach