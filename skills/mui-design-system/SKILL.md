---
name: mui-design-system
description: Use when building, reviewing, or discussing a Material UI (MUI) frontend — any React UI that should follow Material Design via the MUI component library. Covers the full MUI component inventory (inputs, data display, feedback, surfaces, navigation, layout, plus MUI X data grid / date pickers / charts), common app patterns, the Material colour / typography / spacing / elevation / shape system and theming, and how to consume MUI (installation, ThemeProvider, sx / styled, theme tokens) with sensible defaults.
---

# Material UI (MUI)

Help developers build interfaces with **Material UI (MUI)** — the React component library that implements Google's **Material Design** — so screens look consistent, are accessible by default, and are quick to assemble from vetted components. MUI is the default design system for **consumer and general-purpose web apps**: it is broad, familiar, and batteries-included.

MUI is a React library (`@mui/material`, styled with Emotion by default). It is MIT-licensed and free to use, ship, and modify. There is no branding restriction: **theme it to your own brand** — the point of MUI is a consistent, accessible component system, not a fixed visual identity.

## Core principles (apply these first)

1. **Use an MUI component if one exists.** MUI has a vetted, accessible component for almost every common need — buttons, text fields, selects, dialogs, tables, menus, tabs, and more. Reach for it before hand-rolling `<button>`/`<input>`/`<div onClick>`. The full inventory is in `references/components.md` — this is also the conformance inventory for a "no ad-hoc components" check.
2. **Theme, don't hardcode.** Set your palette, typography, shape and spacing once in a `createTheme` and consume tokens (`theme.palette.*`, `theme.spacing`, `theme.typography.*`) via the `sx` prop or `styled()`. Never scatter hex colours or magic pixel values — that breaks dark mode and the type/spacing scale. See `references/styles.md`.
3. **One theme provider, one baseline.** Wrap the app in a single `ThemeProvider` and render `<CssBaseline />` once for a consistent, normalised starting point. See `references/frontend-conventions.md`.
4. **Compose with patterns, not just components.** A form, a dialog flow, an app-bar-plus-drawer shell, a data table with toolbar, a settings page — these are *patterns* built from components. See `references/patterns.md`.
5. **Accessibility is not optional.** MUI components are accessible out of the box, but only if used correctly: label your `TextField`s, give icon-only `IconButton`s an `aria-label`, label your `Dialog`s, and never convey status by colour alone. Pair this with the `mui-accessibility` skill when auditing.

## Bundled references — read the relevant one before answering in detail

| File | When to read it |
| --- | --- |
| `references/components.md` | Choosing/implementing any UI element. Enumerates the MUI component inventory by category (inputs, data display, feedback, surfaces, navigation, layout, utils) plus the MUI X packages (Data Grid, Date/Time Pickers, Charts, Tree View), with each component's purpose, when (not) to use it, and its accessibility requirements. |
| `references/patterns.md` | Designing a screen or a flow — forms, dialogs, app shell/navigation, data tables, feedback, responsive layout, auth screens, settings, empty/loading/error states. |
| `references/styles.md` | The Material system: colour palette & modes (light/dark), typography scale, spacing (8px unit), elevation/shadows, shape/radius, breakpoints, and the theme object. |
| `references/frontend-conventions.md` | Installing MUI, `ThemeProvider` + `CssBaseline`, the styling engines (`sx`, `styled`, `theme`), per-path imports/tree-shaking, MUI X, icons, and using MUI with Next.js / Vite / TypeScript. |

These are distilled from the live MUI documentation (<https://mui.com/>) and Material Design (<https://m3.material.io/>). They are a fast index — for exact, current props and behaviour always confirm against the MUI component API pages, especially across major versions (MUI has evolved its Grid, styling engine, and theme API between v4, v5, v6 and v7).

## Working rules

- **Don't hardcode hex colours or pixels.** Use `theme.palette.primary.main`, `theme.palette.error.main`, `theme.spacing(2)`, and the typography variants, so the UI tracks the theme and dark mode. See `references/styles.md`.
- **Prefer per-path imports** (`import Button from '@mui/material/Button'`) or rely on the library's tree-shaking, so bundle size stays reasonable.
- **Forms:** every `TextField` needs a `label` (or `aria-label`); use `helperText` for hints and `error` + `helperText` for validation messages. Group radios in a `RadioGroup` with a `FormLabel`. Don't use placeholder text as the only label.
- **Icon-only controls:** an `IconButton` or `Fab` that shows only an icon **must** have an `aria-label` — otherwise it has no accessible name.
- **Dark mode & branding:** switch modes with `palette.mode` and brand via the palette — don't fork components or override with `!important`. Keep components unmodified in behaviour; restyle through the theme.
- When starting a new app, set up the theme, `CssBaseline`, and a light/dark toggle first, then build screens from components.

## Related

- `mui-accessibility` skill — WCAG 2.2 AA criteria, how to test, and the accessibility-statement template.
- Commands: `/mui:component`, `/mui:review`, `/mui:audit`, `/mui:preview`.
- `preview/index.html` — a self-contained static kitchen-sink showcasing the core components.
