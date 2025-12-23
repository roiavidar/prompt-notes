# Testing Guidelines

> **Stack Context:** These guidelines use Jest and React Testing Library. Adapt to your testing framework as needed.

**Philosophy**: Test critical functionality only. Confidence over coverage.

**Framework**: `src/test-utils.tsx` • Jest • React Testing Library • Apollo MockedProvider

---

## What to Test

**✅ Test**: Complex logic, edge cases, custom hooks, critical validation, workflows, multi-step forms, Apollo cache interactions, permissions, error handling  
**❌ Skip**: Simple components, trivial wrappers, minor UI, simple CRUD, pure rendering without logic

## Setup

**Always use `customRender`**:
```typescript
import { customRender, screen } from 'src/test-utils'

const { user } = customRender(<Component />, {
  mocks: [{ request: { query: GET_DATA }, result: { data: mockData } }],
  permissions: ['PERMISSION_NAME'],
  theme: customTheme,
  router: { initialEntries: ['/path'] }
})

await user.click(screen.getByRole('button'))
expect(await screen.findByText(/success/i)).toBeInTheDocument()
```

**Helpers**: `setAutocompleteValue(testId, value)`, `clickTableExpandButton(tableTestId)`

## Structure

**One describe per file**: Single top-level `describe`, flat structure preferred  
**Use `test` not `it`**: Always `test()` for test cases  
**Explicit types**: All variables need type annotations (avoid `any`)  
**No comments**: Self-documenting test names  
**No React import**: Unless using React-specific APIs

```typescript
// ✅ DO
const userMock: { __typename: "User" } & User = mockUser({ id: 1 })
let modalOpen: boolean = false

describe('ComponentName', () => {
  test('validates and submits', () => { ... })
})
```

## Best Practices

**Queries**: Prefer `getByRole`, `getByLabelText`, `getByText` for accessibility; use `getByTestId` when needed for clarity  
**Adding testids**: Can add data-testid to components when it improves test readability, but avoid changing component logic/structure for tests  
**Async**: `await screen.findByText()`  
**Mocking**: Module boundary (GraphQL, API), not internal components  
**Test Data**: Reusable → `testObjects.ts`, test-specific → inline  
**Extract repeated queries**: 2+ uses → `const field: HTMLElement = getByTestId("field")`  
**Minimal assertions**: Only critical expectations  
**Cleanup**: `afterEach(() => { cleanup(); jest.clearAllMocks() })`

## Common Pitfalls

- Testing implementation (state, classNames) vs. user behavior
- Snapshot testing everything
- Complex setup (> 20 lines = extract helper)

## Coverage

No target percentage. Measure by deployment confidence.
