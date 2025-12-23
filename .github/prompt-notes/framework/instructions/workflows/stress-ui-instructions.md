# @stress-ui Command

## Objective
Generate comprehensive mock data to stress test UI components with all data scenarios, edge cases, and boundary conditions.

---

## Input
1. `@stress-ui <component-name>` - Searches workspace
2. `@stress-ui <file-path>` - Direct path
3. `@stress-ui [attach file]` - Attached component

---

## Workflow

### Phase 1: Analyze Component

**Read completely**, identify:
- Data props, GraphQL types/interfaces/enums
- Conditional renders, data branches
- UI: tables, lists, chips, forms, modals, cards
- State: hooks, Apollo cache, interactions

**Component Types**:
- **Tables**: Pagination, sorting, filtering, cell renderers, row actions, empty state
- **Lists**: Overflow (+N, scroll), grouping, nesting, drag-drop
- **Forms**: Input types, validation, dependencies, error states
- **Text**: maxHeight/Width, truncation, wrapping, scrolling
- **Modals**: Sizes, scrolling, validation, multiple actions
- **Cards**: Expandable, overflow, fixed/dynamic heights

**Stress Scenarios**:
1. **Nullability**: All null/undefined permutations
2. **Enums**: Every value (status, severity, type)
3. **Arrays**: Empty (0), single (1), small (2-5), medium (6-15), large (50+)
4. **Text**: Empty, short (5-10), normal (20-50), long (100-200), very long (500+)
5. **Numbers**: Zero, negative, large, decimals, percentages (0%, 100%, >100%)
6. **Dates**: Past/present/future, edge dates, time zones
7. **Edge Cases**: Special chars `<>&"'/\`, Unicode, whitespace, injection attempts
8. **Visual**: Unbreakable strings (URLs, IDs), nested levels, mixed lengths
9. **States**: Loading+data, error+partial, empty+filters, all selections active

**Tables**: 2-3 pages (30-50 rows/page), vary data across pages

### Phase 2: Present Strategy

```markdown
## 🎯 Stress Test Strategy for <ComponentName>

### Data Permutations
1. **<Field>**: Scenarios + edge cases
2. **Pagination**: X rows, Y pages
3. **Text**: Fields with 200+ chars
4. **Visual**: Arrays for +N, scrolling

### Mock Plan
- Items: <N>
- Enums: All values
- Nulls: <X> items
- Long text: <Y> items
- Edges: Empty arrays, special chars

**Proceed? (y/n)**
```

### Phase 3: Generate Mock

```typescript
// ============== STRESS TEST MOCK - START (REMOVE AFTER TESTING) ==============
const STRESS_TEST_MOCK: <Type> = {
  items: [
    // Page 1: High enum, long text
    { id: 1, name: "VeryLongNameToTestWrapping...", type: EnumType.High, ... },
    // Page 2: Low/Medium, nulls
    { id: 26, name: "Normal", type: EnumType.Low, field: null, ... },
    // Edge: Empty arrays, special chars
    { id: 50, tags: [], desc: "Special: <>&\"'/\\", ... },
  ]
}
// ============== STRESS TEST MOCK - END ==============

const MyComponent = ({ data }: Props) => {
  // ============== USING STRESS TEST MOCK (REMOVE AFTER TESTING) ==============
  const displayData = STRESS_TEST_MOCK
  // const displayData = data  // Uncomment for real data
  // ============== END MOCK ==============
}
```

**Add imports**: `import { EnumType, ... } from "generated/graphql"`

---

## Verification

**Tables**: Multiple pages, pagination, enum styling, sorting, wrapping, nulls, row actions, empty/loading states
**Lists**: Empty UI, +N overflow, 50+ performance, grouping, nesting
**Forms**: All fields, validation, long inputs, dependencies, errors, submit, required/optional
**Text**: Scrolling, wrapping, special chars safe, no URL overflow, ellipsis, empty fallback
**Modals**: Content scrolls, long titles, actions visible, validation, closes
**Cards**: Expandable toggle, overflow scrolls, heights, grid layout
**Visual**: No breaks, scrollbars, no horizontal overflow, responsive, loading/error states

---

## Examples

```bash
@stress-ui DataTable.tsx
# 50 items, all statuses, long titles, nulls
# Tests: pagination, chips, wrapping, empty state

@stress-ui ItemList.tsx
# 75 items (3 pages), all types, long names
# Tests: multi-page, sorting, filtering, actions

@stress-ui UserForm.tsx
# All field types, validation, long inputs, special chars
# Tests: validation, dependencies, errors, submit

@stress-ui ConfirmDialog.tsx
# Long paths, many options, validation errors
# Tests: modal scroll, autocomplete, form in modal

@stress-ui ItemCard.tsx
# Very long descriptions, many tags, nested data
# Tests: expandable, overflow, scrolling

@stress-ui SummaryCards.tsx
# Many data points, extreme values, edge cases
# Tests: rendering, layout, responsive, overflow

@stress-ui InfoCard.tsx
# Complex nested data, long text fields, many items
# Tests: expandable sections, text truncation, scrolling
```

---

## Common Pitfalls

**Data**: Missing required fields, nested nulls, empty/zero as falsy, negatives
**UI**: Text without spaces, long URLs/UUIDs, Unicode/RTL, deep nesting, circular refs
**Interactions**: Double-clicks, rapid submits, select all large lists, all filters, sort nulls
**Performance**: 100+ items no virtualization, complex calcs per item, heavy re-renders, large nested objects

---

## Guidelines
Follow `${PROMPTS_DIR}/guidelines/architecture-guidelines.md`, `${PROMPTS_DIR}/guidelines/testing-guidelines.md`, `${PROMPTS_DIR}/guidelines/deep-thinking-guidelines.md`. Use `generated/mocks` utilities.

- Match existing mock patterns
- Ensure TypeScript type safety
