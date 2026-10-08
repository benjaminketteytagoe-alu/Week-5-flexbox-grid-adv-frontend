---
sessionId: session-261008-200357-1etk
---

# Requirements

### Overview & Goals

This project is a learning activity that guides students through creating the same page layout using both **Flexbox** and **CSS Grid** across 4 progressive levels. The goal is to understand when and how to use each layout system effectively.

### Current Issues Found

After thoroughly reviewing the README.md instructions against the actual implementation, I identified the following issues:

1. **Level 1 is completely missing** — The file `_level1.scss` exists but is empty (0 lines of code). It is **not imported** in either `flexbox-style.scss` or `grid-style.scss`. When a user visits `?level=1`, they see an unstyled page with no layout at all.

2. **Level 2 does not match the README spec** — The README says Level 2 should have:
   - Header (full width)
   - Navigation menu (full width)
   - Main content area (70% width) with **sidebar (30% width)** — a single right sidebar
   - Footer (full width)
   
   But the current implementation in `_level2.scss` uses **both left and right sidebars** (200px each), which is a Level 3 pattern. The README specifies a single sidebar at 30% width, not two fixed-width sidebars.

3. **Level 2 is missing styling for header, nav, and footer** — Unlike Levels 3 and 4, Level 2 has no appearance styling for `.site-header`, `.site-nav`, `.site-footer`, `.card`, etc. The layout structure is there but the visual presentation is bare.

4. **Level 2 missing responsive nav and footer** — The responsive breakpoint in Level 2 only handles `.page-body` and `.cards`, but doesn't stack the nav items or footer sections on mobile.

### What Needs to Be Done

| Level | Status | Action Needed |
|-------|--------|---------------|
| Level 1 | ❌ Missing | Implement Flexbox and Grid layouts (header/main/footer), add imports |
| Level 2 | ⚠️ Partial | Fix layout to match spec (single sidebar at 30%), add appearance styling, add responsive nav/footer |
| Level 3 | ✅ Complete | No changes needed |
| Level 4 | ✅ Complete | No changes needed |

### Expected Output After Fix

- **Level 1 Flexbox**: Body as flex column, header/footer with fixed height, main content grows to fill space. Stacks on mobile.
- **Level 1 Grid**: Body as grid with `grid-template-rows: auto 1fr auto`. Stacks on mobile.
- **Level 2 Flexbox**: Header, nav, then a flex row of main (70%) + right sidebar (30%), then footer. Stacks on mobile.
- **Level 2 Grid**: Header, nav, then a grid row of `7fr 3fr` for main + sidebar, then footer. Stacks on mobile.

# Technical Design

### Current Implementation

The project uses **SCSS** compiled to CSS via the `sass` CLI (`package.json` scripts). The entry points are:
- `src/scss/flexbox-style.scss` → `dist/flexbox-style.css`
- `src/scss/grid-style.scss` → `dist/grid-style.css`

The `index.html` uses URL query params (`?layout=flexbox&level=1`) to switch between layouts and levels. It sets `data-layout` attribute and `level-N` class on `<body>`.

### Key Decisions

1. **Level 1 will be a shared module** (`_level1.scss`) with both Flexbox and Grid variants, similar to how Level 2 is structured — using `body[data-layout="flexbox"].level-1` and `body[data-layout="grid"].level-1` selectors.

2. **Level 2 will be corrected to match the README**: main content at 70% and a single right sidebar at 30%. The left sidebar will be hidden for Level 2 using `display: none` on `.sidebar-left`.

3. **Level 2 will get appearance styling** (header, nav, footer, cards) consistent with Levels 3 and 4, using the same CSS custom properties pattern.

### Proposed Changes

#### 1. Implement Level 1 (`_level1.scss`)

**Flexbox variant:**
- `body` as `display: flex; flex-direction: column; min-height: 100vh`
- `.site-header` and `.site-footer` with fixed height (e.g., `height: 60px`)
- `.main-content` with `flex-grow: 1`
- Hide nav and sidebars for Level 1 (they're not part of the spec)
- Mobile: stack via `flex-direction: column` (already the default direction)

**Grid variant:**
- `body` as `display: grid; grid-template-rows: auto 1fr auto; min-height: 100vh`
- Named areas: `'header' 'main' 'footer'`
- Hide nav and sidebars
- Mobile: single column grid

**Appearance styling:**
- Add basic header/footer styling (dark background, white text)
- Add card styling consistent with other levels

**Import it:**
- Add `@use 'levels/level1'` to both `flexbox-style.scss` and `grid-style.scss`

#### 2. Fix Level 2 (`_level2.scss`)

**Layout corrections:**
- Hide `.sidebar-left` for Level 2 (README specifies only a right sidebar)
- Change Flexbox: `.main-content` to `flex: 0 0 70%`, `.sidebar-right` to `flex: 0 0 30%`
- Change Grid: `grid-template-columns: 7fr 3fr` with only two columns

**Add appearance styling:**
- Add header styling (dark bg, white text, padding)
- Add nav styling (accent bg, horizontal menu)
- Add footer styling (dark bg, 2-column layout for sections)
- Add card styling (bg, border, border-radius)
- Add CSS custom properties for consistency

**Add responsive nav and footer:**
- Mobile: stack nav items vertically
- Mobile: stack footer sections vertically

#### 3. Rebuild CSS

Run `npm run sass:build` to regenerate `dist/flexbox-style.css` and `dist/grid-style.css`.

### File Structure

```
src/scss/
├── abstract/
│   ├── _reset.scss          (no changes)
│   └── _variables.scss      (no changes)
├── levels/
│   ├── _level1.scss         ← IMPLEMENT (currently empty)
│   ├── _level2.scss         ← FIX layout, add styling
│   ├── _level3.scss         (no changes)
│   ├── _level3-flex.scss    (no changes)
│   ├── _level3-grid.scss    (no changes)
│   ├── _level4-base.scss    (no changes)
│   ├── _level4-flex.scss    (no changes)
│   └── _level4-grid.scss    (no changes)
├── flexbox-style.scss       ← ADD @use 'levels/level1'
└── grid-style.scss          ← ADD @use 'levels/level1'

dist/
├── flexbox-style.css        ← REBUILD
└── grid-style.css           ← REBUILD
```

# Delivery Steps

### ✓ Step 1: Implement Level 1 layout (Flexbox + Grid)
Implement the missing Level 1 basic layout in `_level1.scss` with both Flexbox and Grid variants, and wire it into the build.
- Write Flexbox variant: `body` as flex column with `min-height: 100vh`, header/footer with fixed height, main content with `flex-grow: 1`, hide nav and sidebars.
- Write Grid variant: `body` as grid with `grid-template-rows: auto 1fr auto`, named areas for header/main/footer, hide nav and sidebars.
- Add appearance styling: dark header/footer, card styling, consistent with other levels.
- Add responsive mobile breakpoint that stacks all sections.
- Add `@use 'levels/level1'` to both `flexbox-style.scss` and `grid-style.scss`.
- Run `npm run sass:build` to regenerate dist CSS.

### ✓ Step 2: Fix Level 2 layout and add complete styling
Correct Level 2 to match the README spec (single right sidebar at 30%) and add missing appearance styling.
- Hide `.sidebar-left` for Level 2 (README specifies only a right sidebar).
- Fix Flexbox layout: `.main-content` at `flex: 0 0 70%`, `.sidebar-right` at `flex: 0 0 30%`.
- Fix Grid layout: `grid-template-columns: 7fr 3fr` with only two columns.
- Add appearance styling for header (dark bg, white text), nav (accent bg, horizontal menu), footer (dark bg, 2-column sections), and cards (bg, border, border-radius).
- Add CSS custom properties (`--gap`, `--pad`, `--card-min`, colors) for consistency with Levels 3 and 4.
- Add responsive styling for nav (stack items on mobile) and footer (stack sections on mobile).
- Run `npm run sass:build` to regenerate dist CSS.