## Testim.io Test Generation

> **Note:** This workflow is specific to Testim.io E2E testing tool. Adapt the patterns for your E2E testing framework (Playwright, Cypress, Selenium, etc.).

Generate Testim.io test specs from QA manual tests or feature descriptions.

## Input Sources

1. **Explicit path**: `@testim ${PLAN_PATH}`
2. **Current PLAN.md**: `@testim` (uses `${PLAN_PATH}` if exists)
3. **Specific flow**: `@testim login flow`
4. **Inline steps**: User provides test list

## Test Extraction

**From PLAN.md**: Extract "QA Manual Tests" section, critical flows from Implementation Outline

**Prioritize** (max 3-5 tests):
- ✅ Critical workflows (login, data submission, configuration)
- ✅ Integration between features
- ✅ Permission-gated functionality
- ❌ Skip: Unit validations, edge cases, developer checks

## Test Structure

```json
{
  "testName": "User-perspective description",
  "testId": "kebab-case-id",
  "description": "What this validates",
  "tags": ["feature", "smoke", "critical"],
  "steps": [
    {
      "stepNumber": 1,
      "action": "Navigate to URL",
      "target": "https://app.example.com/path",
      "description": "Step description",
      "validation": "Expected outcome"
    }
  ],
  "preconditions": ["User has permission X"],
  "expectedResults": ["Modal opens", "Data saved"]
}
```

## Actions

`Navigate to URL`, `Click element`, `Type text`, `Select dropdown option`, `Verify element visible`, `Verify element not visible`, `Verify text content`, `Wait for element`, `Wait for navigation`, `Hover over element`, `Press key`

## Selectors (Priority Order)

1. `[data-testid='name']` - Best
2. `[aria-label='desc']` - Good
3. `button:contains('Text')` - OK
4. `.className` - Avoid (brittle)

## Output

**Save to**: `.github/testim/` folder  
**Naming**: `kebab-case-description.json`  
**Create**: README.md index of all tests

**README.md format**:
```markdown
# Testim.io Test Specifications

## Available Tests
### Test Name
**File**: `test-file.json`  
**Description**: What it tests  
**Tags**: `tag1`, `tag2`  
**Steps**: 8

## Importing
1. Testim.io → Tests → Import → JSON
2. Select file from `.github/testim/`
3. Review selectors, run test
```

## Workflow

1. Identify source (file path or PLAN.md QA section)
2. Extract 3-5 critical flows
3. Generate JSON per flow with proper selectors
4. Create `.github/testim/` folder + README.md
5. Present summary with next steps

## Quality Checklist

- [ ] Unique testId per test
- [ ] Sequential step numbering
- [ ] data-testid selectors when available
- [ ] Specific validations
- [ ] Documented preconditions
- [ ] Real user workflows (not implementation details)
- [ ] Realistic timeouts (5-10s)
- [ ] README.md updated

## What to Test

✅ End-to-end journeys, critical workflows, permission-gated features, data persistence, multi-step forms  
❌ Unit validations, code checks, console errors, granular UI states
