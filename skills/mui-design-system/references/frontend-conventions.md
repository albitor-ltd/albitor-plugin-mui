Source: Material UI (MUI) documentation (https://mui.com/). MUI is MIT-licensed. Distilled reference — verify against the live installation/usage pages for current detail, especially across major versions.

# MUI conventions: how to actually build it

Material UI (MUI) is a React component library that implements Material Design. This file covers installing it, the theme provider and baseline, the styling engines (`sx`, `styled`, theme), per-path imports/tree-shaking, icons, the MUI X packages, and using MUI with Next.js / Vite / TypeScript.

## Installing

MUI's default styling engine is **Emotion**:

```bash
npm install @mui/material @emotion/react @emotion/styled
```

Icons (optional but common) and MUI X packages are separate:

```bash
npm install @mui/icons-material
npm install @mui/x-data-grid          # data grid
npm install @mui/x-date-pickers dayjs # date/time pickers (+ a date adapter)
npm install @mui/x-charts             # charts
```

**Roboto font** (Material's default typeface) is not bundled — load it yourself (self-hosted via `@fontsource/roboto`, or your own brand font set in `typography.fontFamily`):

```bash
npm install @fontsource/roboto
```

```js
import '@fontsource/roboto/400.css';
import '@fontsource/roboto/500.css';
import '@fontsource/roboto/700.css';
```

(styled-components is supported as an alternative engine, but Emotion is the default and the simplest path — pick one and stay consistent.)

## Theme provider and baseline (set up once)

Wrap the app in a single `ThemeProvider` and render `CssBaseline` once:

```jsx
import { createTheme, ThemeProvider, CssBaseline } from '@mui/material';

const theme = createTheme({
  palette: {
    mode: 'light',
    primary: { main: '#1976d2' },
    secondary: { main: '#9c27b0' },
  },
  typography: { fontFamily: '"Roboto", "Helvetica", "Arial", sans-serif' },
  shape: { borderRadius: 8 },
});

export default function App() {
  return (
    <ThemeProvider theme={theme}>
      <CssBaseline />
      {/* app */}
    </ThemeProvider>
  );
}
```

`CssBaseline` normalises browser styles and applies the theme's background/text colours — use it instead of a hand-written reset. For dark mode, drive `palette.mode` from state (persist the choice); or adopt MUI's CSS-variables theming (`cssVariables: true` + `colorSchemes`) to support both schemes without a flash — confirm the API for your version.

## Styling: sx, styled, and the theme

Three idiomatic ways to style, all reading from the theme:

- **`sx` prop** — one-off, co-located styling with theme-aware shorthands and responsive objects:
  ```jsx
  <Box sx={{ p: 2, mt: 3, bgcolor: 'background.paper', color: 'text.secondary',
             display: { xs: 'block', md: 'flex' }, gap: 2 }} />
  ```
- **`styled()`** — reusable styled components:
  ```jsx
  import { styled } from '@mui/material/styles';
  const Hero = styled('section')(({ theme }) => ({
    padding: theme.spacing(4),
    background: theme.palette.primary.main,
    color: theme.palette.primary.contrastText,
    borderRadius: theme.shape.borderRadius,
  }));
  ```
- **Theme `components` overrides** — set default props / style overrides for a component library-wide (see `styles.md`).

Always consume tokens (`theme.palette.*`, `theme.spacing`, `theme.typography.*`, `theme.shape`) — never hardcode hex/px. In `sx`, numeric `p`/`m`/`gap` are spacing multiples; colour strings like `'primary.main'` resolve against the palette.

## Imports and bundle size

Prefer **per-path imports** so bundlers tree-shake cleanly:

```js
import Button from '@mui/material/Button';
import TextField from '@mui/material/TextField';
```

Top-level named imports (`import { Button } from '@mui/material'`) also work and modern bundlers tree-shake them, but per-path is the safe default. Icons are always per-path: `import DeleteIcon from '@mui/icons-material/Delete'` (the barrel is huge). Do not import the whole icon package.

## Icons

Material icons ship as React components in `@mui/icons-material` in five themes (Filled, Outlined, Rounded, TwoTone, Sharp — suffix the name, e.g. `DeleteOutlined`). A **decorative** icon should be hidden from assistive tech; an icon that carries meaning needs a text alternative — usually via the surrounding control's `aria-label` (an `IconButton` with only an icon must have one).

## Accessibility conventions (build them in)

- Give every `TextField` a `label` (or `aria-label`); use `helperText` for hints and `error` + `helperText` for validation.
- Every icon-only `IconButton`/`Fab` gets an `aria-label`.
- Label dialogs: `aria-labelledby` → the `DialogTitle` id, `aria-describedby` → the content id.
- Use `Typography component=…` to keep heading semantics correct while choosing a visual size.
- Keep MUI's built-in focus indicators and keyboard behaviour; don't strip them.
- Pair status colour with text/icon; verify contrast after theming.
See the `mui-accessibility` skill for the full WCAG 2.2 AA checklist.

## Using MUI with frameworks

- **Vite / CRA (SPA):** the `ThemeProvider` + `CssBaseline` setup above is all you need. Emotion works out of the box.
- **Next.js (App Router):** MUI components are client components — mark files `'use client'` where needed and use MUI's Next.js integration (`@mui/material-nextjs`) so Emotion's styles are injected correctly during SSR (avoids a flash of unstyled content). Put `ThemeProvider`/`CssBaseline` in a client root and, for the App Router, use `AppRouterCacheProvider`.
- **TypeScript:** MUI is fully typed. To add custom theme tokens (extra palette entries, custom variants), augment the theme types via module augmentation so `theme.palette.myBrand` is typed.

## Keeping it idiomatic

- One `ThemeProvider`, one `CssBaseline`, one styling engine (Emotion) — don't mix engines.
- Reach for an existing component before hand-rolling markup (see `components.md`); reach for MUI X before hand-building a complex data grid, date picker, or chart.
- Theme to brand; don't fork components or fight them with `!important`.
- Pin and track your MUI major version — the Grid API, styling engine defaults, and theme options have changed between v4, v5, v6 and v7; confirm snippets against the version in `package.json`.
