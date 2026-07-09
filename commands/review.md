---
description: Review frontend changes against both Material UI (MUI) usage and WCAG 2.2 AA.
argument-hint: "[file/dir path, or leave blank for the current changes]"
---

Review the MUI frontend in scope: **$ARGUMENTS** (if empty, review the current uncommitted changes; fall back to recently edited frontend files).

Load **both** skills: `mui-design-system` and `mui-accessibility`. Then review on two axes:

1. **MUI / Material Design conformance** (`references/components.md`, `patterns.md`, `styles.md`):
   - Are vetted MUI components used where one exists, instead of bespoke markup (`<button>`/`<input>`/`<select>` that should be `<Button>`/`<TextField>`/`<Select>`)?
   - Are components used idiomatically (correct props/variants, `MenuItem` inside `Select`, `ListItemButton` for interactive list rows) rather than fought against?
   - Is styling done with theme tokens (`sx` + `theme.palette`/`theme.spacing`/`theme.typography`) instead of hardcoded hex/px, so light/dark mode and the scale hold?
   - Is there a single `ThemeProvider` + `CssBaseline` at the root, and one consistent styling engine (Emotion)?

2. **Accessibility** (`references/wcag-2.2.md`): run the WCAG 2.2 AA checks — alt text and icon-button `aria-label`, form labels/helper text, focus visibility, heading order, contrast, `lang`, announced + linked errors, target size, keyboard operability, dialog labelling.

Report findings grouped by axis, each with `file:line`, severity (Blocker/Major/Minor), the problem, and a concrete fix. Note any issue that needs a real-browser or assistive-tech check rather than static review. Finish with a prioritised to-do list. Be specific and code-level; don't restate the standards in the abstract.
