# Coding Style Guidelines

> **Stack Context:** These guidelines use React/TypeScript/MUI examples. Adapt patterns to your stack as needed.

## Core Principles
1. Keep code simple
2. No code comments - use clear naming and structure for self-documenting code
3. When reporting errors: Start with "CONVENTION ERROR:" followed by the specific issue

## Rules by Category

### Imports

**Rule 1 - Use Absolute Paths**
- Use full/absolute import paths
- Never use relative paths with `../../` syntax

**Rule 2 - Type Imports**
- Use `import type` syntax for TypeScript types, interfaces, or type aliases
- Improves tree-shaking and clarifies intent

**Rule 3 - Sort Order**
- Default imports first, then named imports
- Within each group: sort by length (shortest to longest)

**Special Exception: React Import**
- `import React from "react"` if exists, must ALWAYS be the first import, regardless of other rules
- **Only import React when needed**: Don't add `import React from "react"` unless you use React-specific APIs (e.g., `React.useState`, `React.JSX.Element`, `React.memo`). Modern JSX transforms don't require it.

**Then follow this order:**

1. **Named Imports**: List all imports that use curly braces `{}`, mixed imports (default + named from the same module), AND `import type` statements. 
    * These should be sorted by import statement length (shorter statements first).
    * Each import should start in a new line. 

   - Examples:
     - `import type { ITheme } from "${PROJECT_MODULE}/theme"`
     - `import useQueryParam, { NumericFields } from "${HOOKS_DIR}/UseQueryParam"`
     - `import Box from "@mui/material/Box"`

2. **Default Imports**: List all imports that don't use curly braces `{}` and are pure default imports only (excluding React). These should also be sorted by import statement length.
   - Examples:
     - `import MyComponent from "./MyComponent"`

**Import Paths**: 
Use full paths instead of relative paths with `../../` syntax. Always use absolute imports from the project root.

**Type Imports**:
When importing TypeScript types, interfaces, or type aliases, use the `import type` syntax for better tree-shaking and clearer intent.
When type imports are with {} consider it as named import for sorting purposes.
This is the recommended TypeScript best practice.

**Always add explicit types to imported values**:
- Add type annotations to imported functions, objects, and variables when their types aren't immediately obvious
- This improves code clarity and catches type errors earlier

- Examples:
    - `import type { ITheme } from "${PROJECT_MODULE}/theme"`
    - `import { type ITheme } from "${PROJECT_MODULE}/theme"`
    - `import { MeDocument, type Permission, type Role } from "generated/graphql"`
    - `const permissions: PERMISSION[] = [PERMISSIONS.PAGE_READ]` (explicit type annotation)

**Path length is measured by counting the number of characters in the entire import statement** (e.g., `"FormControl from \"@mui/material/FormControl\"".length` = 44 characters).

import type is treated the same as a named import for ordering purposes.

**Path Length Examples:**
- `{ Box } from "@mui/material"` = 30 characters (short)
- `type { ITheme } from "${PROJECT_MODULE}/theme"` = 44 characters (medium)  
- `useQueryParam, { NumericFields } from "${HOOKS_DIR}/UseQueryParam"` = 69 characters (long)

❌ DON'T

```
import { useTheme } from "@mui/material/styles"
import { makeStyles } from "${PROJECT_MODULE}/makeStyles"
import Box from "@mui/material/Box"
import Typography from "@mui/material/Typography"
import React from "react"
import utils from "../../../utils/helpers"
import MyComponent from "../../components/MyComponent"
import { ITheme, UserRole } from "${PROJECT_MODULE}/theme"
```

✅ DO

```
import React from "react"
import { Box } from "@mui/material"
import { useTheme } from "@mui/material/styles"
import { MUIDataTableColumn } from "mui-datatables"
import { makeStyles } from "${PROJECT_MODULE}/makeStyles"
import type { ITheme, UserRole } from "${PROJECT_MODULE}/theme"
import useQueryParam, { NumericFields } from "${HOOKS_DIR}/UseQueryParam"
import utils from "${UTILS_DIR}/helpers"
import MyComponent from "${PROJECT_MODULE}/components/MyComponent"
```

**Note**: In the correct example above, `{ Box }` (30 chars) appears before `utils` (40 chars) and `MyComponent` (48 chars) because named imports come before default imports, regardless of length. This is correct.

## Tags

Use <> instead of <React.Fragment>. If a key is required, use a real HTML/JSX tag instead.

## Styling

When applying a single CSS property, prefer MUI built-in props (when available) or the style prop for consistency with the MUI styling system.
Do not use sx for single properties.

When applying multiple CSS properties, use a CSS class instead of inline styles or multiple props.

Typography components should always include a `variant` prop for consistent styling and accessibility.

❌ DON'T

```
<Typography sx={{ fontWeight: 600 }} />

<Stack direction="row" alignItems="center" gap={2}>

<Typography>Some text</Typography>

```

✅ DO

```
<Typography fontWeight={600} />
<Box style={{ fontWeight: 600 }} />
<Stack className={classes.stack}>
<Typography variant="body1">Some text</Typography>

```

### TypeScript Types

**Rule 1 - No Void Return Type**
- Functions do not require explicit `void` return type
- Remove `void` when it's the return type

**Rule 2 - Explicit Type Annotations**
- Every variable declared with `const` or `let` must have explicit type annotation
- Every function must have explicit return type annotation

**Examples:**

❌ INCORRECT
```tsx
const handleClick = (): void => {
  // function body
}

const userName = "John"
const getUserData = () => {
  return { name: "John" }
}
```

✅ CORRECT
```tsx
const handleClick = () => {
  // function body
}

const userName: string = "John"
const getUserData = (): { name: string } => {
  return { name: "John" }
}

```

### Naming Conventions

**Rule 1 - Folders**
- Use lowercase with hyphens
- Example: `folder-name`

**Rule 2 - Components**
- Use PascalCase
- Example: `UserProfile`

**Rule 3 - Variables**
- Use camelCase
- Example: `userName`

**Rule 4 - Interfaces**
- Use PascalCase
- Example: `MapPoint`

**Rule 5 - Props Interfaces**
- Use PascalCase with 'Props' suffix
- Example: `MemoryRouterProps`

**Examples:**

❌ INCORRECT
```tsx
// folder: UserProfile
// component: userProfile
// variable: UserName
// interface: mapPoint
// props interface: MemoryRouter
```

✅ CORRECT
```tsx
// folder: user-profile
// component: UserProfile
// variable: userName
// interface: MapPoint
// props interface: MemoryRouterProps

```

### Functions

**Rule - Single-Expression Arrow Functions**
- Use arrow functions without braces for single-expression functions
- Omit explicit return statement

**Examples:**

❌ INCORRECT
```tsx
const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    return onChange(event.target.checked)
}

const calculateTotal = (price: number, tax: number) => {
    return price + tax
}
```

✅ CORRECT
```tsx
const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => onChange(event.target.checked)

const calculateTotal = (price: number, tax: number) => price + tax
```

### Text Constants

Whenever you have a text constant, it should be defined in the `${LOCALE_DIR}/type.ts` with the appropriate type and `${LOCALE_DIR}/en.ts` with the concrete text accordingly. Always try to use existing text constants, and if you need to create a new one, make sure to follow the naming conventions.

## Code Reusability

Whenever possible, try to reuse existing components or functions instead of creating new ones. If you find yourself writing similar code in multiple places, consider extracting it into a reusable component or utility function. Check for existing implementations in `${UTILS_DIR}` folder for relevant utils functions and `${COMMON_DIR}/` folder for common components.
