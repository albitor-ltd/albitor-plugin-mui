---
description: Look up or scaffold a Material UI (MUI) component (JSX, props, accessibility notes).
argument-hint: "[component name or what you're trying to build]"
---

The user wants help with a Material UI (MUI) component: **$ARGUMENTS**

Do the following:

1. Load the `mui-design-system` skill and read `references/components.md` (and `references/patterns.md` if the request describes a flow or screen rather than a single element).
2. Identify the right component(s). If the request is vague (e.g. "let users pick one of a few options"), recommend the correct component and say why (e.g. `RadioGroup` for a few visible options, `Select`/`Autocomplete` for many); mention any component they should *not* reach for (e.g. don't hand-roll a dropdown).
3. Provide a realistic **MUI (React) snippet** with sensible props, importing from the correct package (`@mui/material`, `@mui/icons-material`, or an MUI X package like `@mui/x-data-grid` / `@mui/x-date-pickers` where relevant). Prefer per-path imports (`import Button from '@mui/material/Button'`) so the note on tree-shaking in `references/frontend-conventions.md` holds.
4. Call out the **accessibility requirements** for that component: a `TextField` needs a `label` (or `aria-label`); an icon-only `IconButton`/`Fab` needs an `aria-label`; a `Dialog` needs `aria-labelledby`/`aria-describedby`; status colour must be paired with text/icon (not colour alone); interactive controls must keep visible focus. Reference the relevant WCAG criterion.
5. Show how to style it the MUI way — the `sx` prop or `styled()` with **theme tokens** (`theme.palette.*`, `theme.spacing`, `theme.typography.*`), not hardcoded hex or px, so it tracks the theme and dark mode. See `references/styles.md`.
6. Match the surrounding codebase: detect whether the project already uses MUI (and which version / MUI X packages), TypeScript, Next.js or Vite, and Emotion vs styled-components, and tailor the snippet to it.

If no argument was given, ask what they're building and list the component categories from the reference.
