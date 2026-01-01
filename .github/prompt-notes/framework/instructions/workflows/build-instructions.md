## Build Implementation Workflow

**CRITICAL**: Code implementation + verification only (no test writing).

**FLEXIBLE INPUT**: Works with existing PLAN.md, direct task request, screenshot, or custom plan file.

---

## Input Modes
1. **`@build`** - Uses existing `${PLAN_PATH}`
2. **`@build <request>`** - Direct implementation (optionally creates plan for complex tasks)
3. **`@build [screenshot]`** - Extracts requirements from UI mockup
4. **`@build path/to/plan.md`** - Uses custom plan file

---

## Required Guidelines (load before coding)
1. [`${PROMPTS_DIR}/guidelines/architecture-guidelines.md`](${PROMPTS_DIR}/guidelines/architecture-guidelines.md) - Component patterns, state, GraphQL
2. [`${PROMPTS_DIR}/guidelines/ui-guidelines.md`](${PROMPTS_DIR}/guidelines/ui-guidelines.md) - Spacing, components, responsive layout
3. [`${PROMPTS_DIR}/guidelines/code-style-guidelines.md`](${PROMPTS_DIR}/guidelines/code-style-guidelines.md) - Imports, naming, TypeScript patterns
4. [`${PROMPTS_DIR}/guidelines/error-handling-guidelines.md`](${PROMPTS_DIR}/guidelines/error-handling-guidelines.md) - GraphQL errors, REST errors, retry logic, user feedback

---

## Workflow

### Phase 1: Understand Request
- **Mode 1** (existing PLAN.md): Read + validate completeness
- **Mode 2** (direct request): Simple → implement directly; Complex → optionally create plan first
- **Mode 3** (screenshot): Extract components, spacing, layout, interactions
- **Mode 4** (custom plan): Read specified file

### Phase 2: Implementation
- **New files**: Proper structure (imports → types → component → styles)
- **Modified files**: Surgical changes with 3-5 lines context
- **Pattern alignment**: Match existing patterns, reuse utilities, apply ui-guidelines spacing
- **Track progress**: Update PLAN.md (if exists) OR keep internal checklist

### Phase 3: Verification
1. **TypeScript**: `${TYPECHECK_CMD}` (fix until clean)
2. **Lint**: `${LINT_CMD}` (auto-fix + manual cleanup)
3. **Manual QA**: Test states, interactions, keyboard nav, permissions
4. **Locale** (if user-facing): Add to type.ts, en.ts

### Phase 4: Completion
1. **Update PLAN.md** (if exists): Mark Definition of Done items
2. **Cleanup**: Remove unused imports, console.logs, TODOs
3. **Summary**: Files changed, verification status, key decisions, ready for what
4. **Prompt user next steps**:
   ```
   ✅ Implementation complete
   
   Ready to add tests? Run: @test
   
   Or test specific files: @test <filename>
   
   To identify and eliminate code duplication: @extract-concerns
   ```

---

## Error Recovery
- **Blocked**: Document in PLAN.md with `[⚠️]`, notify user, suggest alternatives
- **Breaking changes**: Use `list_code_usages`, update all affected files, document
- **Quality issues**: Fix TypeScript/lint errors, re-verify

---

## Verification Checklist
- [ ] All changes implemented per plan/request
- [ ] TypeScript clean
- [ ] Lint clean
- [ ] Manual QA complete
- [ ] Locale strings added (if applicable)
- [ ] Summary provided

---

## Key Principles
🎯 **Flexible input** - 4 modes  
📋 **Guidelines-first** - Load all 3 before coding  
✅ **Verify everything** - TypeScript + lint + manual QA  
📝 **Document clearly** - Update PLAN.md or provide summary  
🧪 **Tests separate** - Implementation only

**Goal**: Production-ready code implementation with quality verification, following all guidelines and documenting decisions.
