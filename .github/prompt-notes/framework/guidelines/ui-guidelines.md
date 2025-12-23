# UI Implementation Guidelines

> **Stack Context:** These guidelines use MUI (Material-UI) components and styling patterns. Adapt to your UI library as needed.

**Implementing designs from screenshots with pixel-perfect accuracy: spacing, responsive, components, theme.**

---

## Core Principles

1. **Design Fidelity** - Match design exactly (spacing, colors, typography, alignment)
2. **Responsive First** - Mobile, tablet, desktop breakpoints from start
3. **Component Reuse** - Use existing MUI/custom components from `common/`
4. **Theme Consistency** - Follow theme spacing, colors, typography
5. **Accessibility** - Proper contrast, focus states, semantic HTML

---

## Screenshot Implementation Process

### 1. Initial Analysis

**Goal**: Extract precise measurements and map to components

#### Measurement Extraction

**Measurement Methods** (in order of accuracy):

1. **User-Provided Measurements** (BEST) - User extracts exact px from Figma/design tools, provides in chat
2. **Screenshot Analysis** (GOOD) - AI vision analyzes attached screenshot, extracts visual measurements
3. **Visual Estimation** (OK) - AI infers spacing from screenshot context and similar patterns

**Extraction steps**:
1. **Identify base screen width** - Usually 1280px, 1440px, or 1920px
2. **Measure outer container** - Padding from edges (usually 24px, 48px, or 56px)
3. **Measure section gaps** - Vertical/horizontal space between major sections
4. **Measure component spacing** - Gap between buttons, cards, form fields
5. **Measure internal padding** - Inside cards, dialogs, table cells
6. **Identify alignment** - left/center/right, start/end, space-between

**Create spacing inventory**:
```
Outer padding: 48px → 3rem
Section gap: 24px → 1.5rem
Card padding: 24px → 1.5rem
Button gap: 8px → 0.5rem
Title margin: 16px → 1rem
```

#### Component Mapping

- **Components**: Map to MUI (Box, Stack, Grid, Paper, Typography) • Check `${COMMON_COMPONENTS_DIR}/`
- **Spacing**: Convert pixels to rem (px / 16), round to theme scale
- **Layout**: Grid/Stack/Flexbox • Responsive breakpoints • Container widths
- **Visual**: Colors (theme.palette) • Typography (variant, weight, size) • Border radius, shadows

#### JSON Spec Format

Use this format to document measurements (for @ui-accurate command):

```json
{
  "screen_width_estimate": 1440,
  "spacing_scale": [4, 8, 12, 16, 20, 24, 32, 48],
  "layout": {
    "container": {
      "padding": {"top": 16, "right": 48, "bottom": 0, "left": 48},
      "direction": "column"
    },
    "sections": [
      {
        "name": "header",
        "direction": "row",
        "alignment": "space-between",
        "padding": {"top": 24, "bottom": 16},
        "gap": 16
      }
    ]
  },
  "measurements": {
    "outer_padding": 48,
    "section_gap": 24,
    "card_padding": 24,
    "button_spacing": 8
  }
}
```

---

## Spacing System

### Theme Units (rem-based)

```
0.25 (4px)  | 0.5 (8px)   | 0.75 (12px) | 1 (16px)    | 1.25 (20px)
1.5 (24px)  | 2 (32px)    | 2.5 (40px)  | 3 (48px)    | 3.5 (56px)
```

### Application

```tsx
// ✅ MUI props for single properties
<Box padding="1rem" margin="0.5rem">
<Stack spacing={2} padding="1.5rem">

// ✅ style prop for single CSS property
<Box style={{ paddingTop: "2rem" }}>

// ❌ DON'T use sx for single properties
<Box sx={{ padding: "1rem" }}>

// ✅ Classes for multiple properties
<Box className={classes.container}>
```

### Common Patterns

```tsx
// Page padding
padding: "1rem 3rem 0rem 3rem"  // top right bottom left

// Card padding
<CardContent>                    // Default: 1rem
<Paper style={{ padding: "1.5rem" }}>

// Stack spacing (gap between children)
<Stack spacing={2}>              // 16px gap
<Stack spacing={1.5}>            // 12px gap

// Grid spacing
<Grid container spacing={2}>    // 16px gap
<Grid container columnSpacing={1} rowSpacing={2}>
```

---

## Responsive Design

### Breakpoints

```
sm: < 600px  (mobile)  | md: < 900px  (tablet)
lg: < 1200px (desktop) | xl: < 1536px (large)
```

### Patterns

```tsx
// Grid2 responsive
import Grid from "@mui/material/Grid2"
<Grid size={{ xs: 12, sm: 6, md: 4 }}>  // Full→Half→Third

// makeStyles responsive
const useStyles = makeStyles()((theme) => ({
  container: {
    padding: "2rem",
    [theme.breakpoints.down("md")]: { padding: "1rem" },
    [theme.breakpoints.down("sm")]: { padding: "0.5rem" },
  },
  sidebar: {
    width: "14rem",
    [theme.breakpoints.down("lg")]: {
      width: "100%",
      order: 2,  // Reorder on small screens
    },
  },
}))

// Responsive props
<Stack direction={{ xs: "column", sm: "row" }} spacing={{ xs: 1, sm: 2 }}>
<Box display={{ xs: "none", md: "block" }}>  // Hide on mobile
```

---

## Component Selection

**Grid2**: Complex layouts, responsive columns  
**Stack**: Vertical/horizontal with consistent spacing  
**Box**: Generic container  
**Paper**: Elevated surfaces, cards

```tsx
// Page container
<Box className={classes.page}>
  <Grid container spacing={2}>...</Grid>
</Box>

// Card
<Paper style={{ padding: "1.5rem" }}>
  <Stack spacing={2}>
    <Typography variant="h6">Title</Typography>
    <Typography variant="body1">Content</Typography>
  </Stack>
</Paper>

// Form
<Stack spacing={2}>
  <TextField fullWidth />
  <Stack direction="row" spacing={1} justifyContent="flex-end">
    <Button>Cancel</Button>
    <Button variant="contained">Save</Button>
  </Stack>
</Stack>
```

---

## Typography

### Always Include Variant

```tsx
// ❌ DON'T
<Typography>Text</Typography>

// ✅ DO
<Typography variant="body1">Text</Typography>
<Typography variant="h6">Heading</Typography>
```

**Variants**: `h1-h6` (headings) • `body1` (16px) • `body2` (14px) • `caption` (12px) • `button`

### Custom Styles

```tsx
// Single property - use prop
<Typography variant="body1" fontWeight={600}>

// Multiple - use class
const useStyles = makeStyles()((theme) => ({
  title: {
    fontWeight: 600,
    fontSize: "1.25rem",
    color: theme.palette.grey[800],
  },
}))
```

---

## Colors & Visual

### Theme Colors

```tsx
// In makeStyles
const useStyles = makeStyles()((theme) => ({
  container: {
    color: theme.palette.grey[800],
    borderColor: theme.palette.grey[300],
    backgroundColor: theme.palette.common.white,
  },
}))

// Common paths
theme.palette.primary.main
theme.palette.common.white/black
theme.palette.grey[100-900]
theme.palette.success/error/warning.main
theme.palette.backdrop[100-800]
```

### Borders & Shadows

```tsx
borderRadius: "4px"     // Default
borderRadius: "0.5rem"  // 8px - Cards, buttons

<Paper elevation={2}>   // Standard shadow
boxShadow: `0 2px 5px 0 ${theme.palette.action.focus}`  // Custom
```

---

## Implementation Checklist

- [ ] **Components**: Map to MUI/custom, check `common/components/` first
- [ ] **Spacing**: Document in rem, use theme scale
- [ ] **Responsive**: Define xs/sm/md/lg/xl variations
- [ ] **Colors**: Match theme.palette
- [ ] **Typography**: Identify variants, weights, sizes
- [ ] **Layout**: Grid/Stack/Box selection
- [ ] **Test breakpoints**: Verify all sizes
- [ ] **Alignment**: Match design exactly
- [ ] **Accessibility**: Contrast, focus, semantic HTML

---

## Example Implementation

**Design**: Card with white bg, shadow, 24px padding, title (20px bold), subtitle (14px), icon (right), full-width button, responsive stack

```tsx
import React from "react"
import { makeStyles } from "${PROJECT_MODULE}/makeStyles"
import Box from "@mui/material/Box"
import Paper from "@mui/material/Paper"
import Stack from "@mui/material/Stack"
import Button from "@mui/material/Button"
import Typography from "@mui/material/Typography"

const useStyles = makeStyles()((theme) => ({
  card: {
    padding: "1.5rem",
    boxShadow: `0 2px 5px 0 ${theme.palette.action.focus}`,
  },
  header: {
    display: "flex",
    alignItems: "center",
    marginBottom: "1rem",
    justifyContent: "space-between",
  },
  title: {
    fontWeight: 600,
    fontSize: "1.25rem",
    color: theme.palette.grey[800],
  },
  subtitle: {
    fontSize: "0.875rem",
    color: theme.palette.grey[600],
  },
}))

const DesignCard = (): React.JSX.Element => {
  const { classes } = useStyles()
  return (
    <Paper className={classes.card}>
      <Stack spacing={2}>
        <Box className={classes.header}>
          <Typography className={classes.title}>Title</Typography>
          <Icon />
        </Box>
        <Typography className={classes.subtitle}>Subtitle</Typography>
        <Button variant="contained" fullWidth>Action</Button>
      </Stack>
    </Paper>
  )
}
```

---

## Common Mistakes

❌ No responsive breakpoints  
❌ Hardcoded pixels (use rem)  
❌ sx for single properties  
❌ Ignore existing components  
❌ Missing Typography variant  
❌ Inconsistent spacing  
❌ Not testing responsive  
❌ Duplicate components

---

## Resources

- **Theme**: `${COMMON_DIR}/theme.tsx`
- **Components**: `${COMMON_COMPONENTS_DIR}/`
- **Styles**: `${COMMON_DIR}/styles/`
- **Page padding**: `${COMMON_DIR}/styles/usePagePadding.ts`
- **Examples**: `${SRC_DIR}/pages/feature-a/dashboard/`

