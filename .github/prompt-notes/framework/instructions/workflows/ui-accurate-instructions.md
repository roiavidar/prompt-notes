# @ui-accurate - UI Accuracy Refinement

**Purpose**: Refine UI to match screenshots pixel-perfectly after initial implementation.

**Usage**: `@ui-accurate <file-path>` + attach target screenshot

---

## 3-Phase Process

### PHASE 1: ANALYZE

**Extract JSON spec from screenshot**:
```json
{
  "screen_width_estimate": 1440,
  "spacing_scale": [4, 8, 12, 16, 20, 24, 32, 48],
  "layout": {
    "container": {"padding": {"top": 16, "right": 48, "bottom": 0, "left": 48}, "direction": "column", "gap": 16},
    "sections": [{"name": "header", "direction": "row", "alignment": "space-between", "padding": {"top": 24, "bottom": 16}, "gap": 16}]
  },
  "measurements": {"outer_padding": 48, "section_gap": 24, "card_padding": 24, "button_spacing": 8}
}
```

**Analyze current code**: spacing values, layout structure, wrapper count, gap vs margin, alignment

**Detect mismatches** (screenshot vs current):
```markdown
1. Container Padding - Screenshot: 3rem, Current: 1.5rem, Fix: style={{ padding: "1rem 3rem 0 3rem" }}
2. Title Gap - Screenshot: 1rem, Current: 1.5rem, Fix: spacing={2} + marginBottom
3. Button Spacing - Screenshot: 0.5rem, Current: 1rem, Fix: spacing={1}
4. Unnecessary Wrapper - Remove extra Box
```

**Generate visual preview file**: Create `.github/UI_PROPOSAL.tsx` with formatted, easy-to-read proposed structure:
```tsx
// Proposed UI structure for <ComponentName>
// Changes: Container padding (3rem), title gap (1rem), button spacing (0.5rem), removed wrapper

import React from "react"
import Box from "@mui/material/Box"
import Stack from "@mui/material/Stack"
import Button from "@mui/material/Button"
import Typography from "@mui/material/Typography"

export const ProposedComponent = () => (
  <Box style={{ padding: "1rem 3rem 0 3rem" }}>
    <Stack spacing={2}>
      <Typography variant="h5" style={{ marginBottom: "1rem" }}>Title</Typography>
      <Stack direction="row" spacing={1}>
        <Button>Action 1</Button>
        <Button>Action 2</Button>
      </Stack>
    </Stack>
  </Box>
)
```

**Output**: 
1. JSON spec + mismatch list (in chat)
2. `.github/UI_PROPOSAL.tsx` (for visual inspection)
3. **Wait for user approval** before proceeding to Phase 2

---

### PHASE 2: IMPLEMENT

**Apply corrections** following ui-guidelines.md:

**Spacing**:
- Single property → `style` prop or MUI prop: `<Box padding="1.5rem">`
- Multiple properties (2+) → `makeStyles` class
- Use `spacing` for gaps, NOT margins
- Convert: px ÷ 16 = rem → round to theme scale (0.25, 0.5, 0.75, 1, 1.25, 1.5, 2, 2.5, 3, 3.5)

**Layout**:
- Grid2: Complex/responsive columns
- Stack: Linear with spacing
- Box: Generic wrapper
- Paper: Elevated surfaces
- **Minimize wrappers**: Remove unnecessary nesting

**Typography**: Always include `variant` (h1-h6, body1/body2, caption)

**Colors**: Use `theme.palette.*` (grey[100-900], primary.main, etc.)

**Generate**: Complete corrected file with inline spacing comments

---

### PHASE 3: VERIFY

**Checklist**:
- [ ] Container padding matches (top/right/bottom/left)
- [ ] Section gaps match spacing scale
- [ ] Typography variants specified
- [ ] Colors from theme.palette
- [ ] Minimal wrappers
- [ ] No hardcoded pixels (all rem)
- [ ] Responsive breakpoints (xs/sm/md/lg)

**If issues remain**: Generate difference list → apply final corrections

**Output**: Checklist + change summary

---

## Key Rules

**DO**:
- ✅ Gap for spacing: `<Stack spacing={2}>`
- ✅ Single property: `<Box padding="1.5rem">` or `style={{ padding: "1.5rem" }}`
- ✅ Multiple properties: `makeStyles` class
- ✅ rem-based: `1.5rem` not `24px`
- ✅ Theme colors: `theme.palette.grey[800]`

**DON'T**:
- ❌ Margins for sibling spacing (use `spacing`)
- ❌ sx for single property (use `style` or MUI prop)
- ❌ Multiple properties in `style`/`sx` (use class)
- ❌ Hardcoded pixels
- ❌ Missing Typography variant

---

## Common Fixes

**Page padding override**:
```tsx
<Box className={classes.pagePadding} style={{ paddingTop: "1rem" }}>
```

**Stack with title gap**:
```tsx
<Typography variant="h6" style={{ marginBottom: "1rem" }}>Title</Typography>
<Stack spacing={2}><Content /></Stack>
```

**Grid row/column spacing**:
```tsx
<Grid container rowSpacing={1.5} columnSpacing={2}>
```

---

## Success Criteria

✅ Visual match within 2-4px  
✅ Theme-compliant spacing/colors  
✅ Minimal wrappers  
✅ Responsive at all breakpoints  
✅ Accessible (semantic HTML, contrast)

---

**Reference**: `${PROMPTS_DIR}/guidelines/ui-guidelines.md` for complete patterns
