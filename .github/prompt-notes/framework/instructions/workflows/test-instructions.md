# @test Command

## Objective
Intelligent test maintenance based on git changes: add tests for new code, remove obsolete tests, avoid duplication.

**Philosophy**: `testing-guidelines.md` - Test critical only, confidence over coverage.

**Deep Thinking**: `deep-thinking-guidelines.md` - Pattern discovery first.

---

## Input Modes
1. **`@test <filename>`** - Prompts user with test plan for specific file
2. **`@test <file-path>`** - Same with explicit path
3. **`@test`** - Auto-generates tests for all changed files (no prompt)

---

## Workflow

### Phase 1: Understand Changes
**Git diff analysis**:
- Mode 1/2: `git diff <file-path>`
- Mode 3: `git diff --name-only --diff-filter=AM`

**Identify**:
- Lines added → New functionality needing tests
- Lines removed → Obsolete tests to delete
- Context → Logic, UI, GraphQL, utilities

**Locate test file**: `<filename>.test.tsx` (same directory)
- If missing: Create with `customRender` setup (testing-guidelines.md)

### Phase 2: Deep Analysis
**Pattern discovery** (deep-thinking-guidelines.md):
- Search: `semantic_search "test patterns for <component-type>"`
- Document: Similar tests, utilities, mocks, assertions

**What to test** (testing-guidelines.md):

**✅ Critical**:
- Complex logic, edge cases
- Custom hooks, state management
- Workflows (forms, multi-step)
- Apollo cache, permissions, error handling

**❌ Skip**:
- Simple JSX renders
- Trivial wrappers
- Minor UI interactions
- Simple CRUD

**Avoid duplication**:
- Read existing tests
- Check coverage
- Only add if NOT already tested

### Phase 3: User Confirmation (Mode 1/2 only)
**Step 1: Scope Decision**

Ask user first:
```markdown
## 🧪 Test Scope for `<filename>`

**Option 1: Changed Lines Only** (recommended)
- Test only added/removed lines from git diff
- Faster, focused on recent changes
- Maintains existing test coverage

**Option 2: Full Component**
- Test entire component comprehensively
- Longer, ensures complete coverage
- Checks existing tests to avoid duplication

**Which scope? (1/2)**
```

**Step 2: Prompt with test plan**

**If Option 1 (changed lines)**:
```markdown
## 🧪 Test Plan for `<filename>` (Changed Lines Only)

### Changes Detected
**Added**: [Line Y-Z] New `handleSubmit` - complex validation
**Removed**: [Line M-N] Old `validateForm`

### Proposed Tests
**New**: handleSubmit validation, GraphQL mutation, permission check
**Remove**: validateForm tests (lines 45-78)
**Keep**: renders form fields

**Proceed? (y/n)**
```

**If Option 2 (full component)**:
```markdown
## 🧪 Test Plan for `<filename>` (Full Component)

### Component Analysis
**Critical functionality**:
- handleSubmit validation
- Form submission flow
- Permission checks
- Error handling

**Existing tests**: 8 test cases (120 lines)

### Proposed Tests
**Add**: Missing edge cases, error states
**Update**: Existing tests for better coverage
**Remove**: Duplicate/obsolete tests

**Proceed? (y/n)**
```

### Phase 4: Implementation
**Load guidelines**:
1. `testing-guidelines.md` - Setup, best practices
2. `deep-thinking-guidelines.md` - Pattern alignment
3. Similar test files

**Generate tests** (testing-guidelines.md):
```typescript
import { customRender, screen } from 'src/test-utils'

describe('MyComponent', () => {
  it('validates fields on submit', async () => {
    const { user } = customRender(<MyComponent />, {
      mocks: [/* GraphQL */],
      permissions: ['PERMISSION'],
    })
    await user.click(screen.getByRole('button', { name: /submit/i }))
    expect(await screen.findByText(/required/i)).toBeInTheDocument()
  })
})
```

**Best practices**:
- `getByRole`, `getByLabelText`, `getByText` (NOT `getByTestId`)
- `await screen.findByText()` for async
- Mock at boundary (GraphQL, not components)
- Setup < 20 lines

**Remove obsolete tests**:
- Grep for deleted function names
- Remove `describe`/`it` blocks
- Update imports

**Deduplicate**:
- Merge similar tests
- Consolidate redundant setup

### Phase 5: Verification
1. **Run**: `${TEST_CMD} <test-file-path>`
2. **TypeScript**: `${TYPECHECK_CMD}`
3. **Coverage**: Optional - confidence over percentage

### Phase 6: Summary
```markdown
## ✅ Test Updates Complete
**File**: MyForm.tsx
**Changes**: Added 3, Removed 2, Deduped 2
**Results**: 9/9 passing
**Ready for**: Code review
```

---

## Mode 3 Auto-Discovery

**Filter**: `.tsx`, `.ts` (skip `.test`, config, types)

**Process each**:
1. Analyze diff
2. Auto-decide (no prompt): Add critical tests, remove obsolete, skip trivial
3. Implement

**Batch summary**: Files processed, tests added/removed/deduped, all passing

---

## Error Recovery
- **No test file**: Ask to create
- **Tests fail**: Ask fix or skip
- **Cannot determine**: Ask user for context

---

## Key Principles
🧪 **Critical only** - testing-guidelines.md  
🔍 **Patterns first** - deep-thinking-guidelines.md  
🗑️ **Remove obsolete** - Clean deleted code tests  
🚫 **No duplication** - Check existing coverage  
✅ **Confidence > coverage** - Critical paths  
🤖 **Smart auto** - Mode 3 auto-decides

**Goal**: Lean, effective test suite providing confidence without bloat.
