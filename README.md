# Material UI (MUI) plugin (`mui`)

A [Claude Code plugin](https://code.claude.com/docs/en/plugins) that helps you build, audit, and preview **Material UI (MUI)** interfaces — the React component library that implements Google's **Material Design**. It packages the MUI component inventory and Material system (colour, type, spacing, elevation, theming) plus **WCAG 2.2 AA accessibility** into skills, commands, an audit agent, an advisory hook, and a self-contained preview page.

MUI is the **default design system for consumer and general-purpose web apps**: broad, familiar, batteries-included, MIT-licensed, and free to theme to your own brand. This plugin is one of the design-system plugins Albitor makes available to its users; the reference content is distilled from the live MUI and Material Design docs and bundled so the assistant has authoritative material to hand (see [Licensing](#licensing)).

## What's inside

### Skills (load automatically when relevant)

| Skill | Triggers on | Bundled references |
| --- | --- | --- |
| `mui:mui-design-system` | Building/reviewing a Material UI / React frontend | The MUI component inventory (inputs, data display, feedback, surfaces, navigation, layout, utils + MUI X data grid / pickers / charts), common app patterns, the Material colour/type/spacing/elevation/shape system, and how to consume MUI |
| `mui:mui-accessibility` | Building/testing/advising on accessibility | All 50 WCAG 2.2 A/AA criteria (with MUI-specific how-to-meet notes), how to test, and a consumer-product accessibility-statement template |

### Commands

| Command | Does |
| --- | --- |
| `/mui:component [name or need]` | Look up or scaffold an MUI component — idiomatic React snippet with theme tokens and accessibility notes. |
| `/mui:audit [target]` | WCAG 2.2 AA audit of a file, component, or the current changes; prioritised findings. |
| `/mui:review [target]` | Review frontend changes against **both** correct MUI usage and WCAG 2.2 AA. |
| `/mui:accessibility-statement [details]` | Draft an accessibility statement for a consumer/general-purpose product. |
| `/mui:preview [what]` | Open, explain, or derive from the self-contained MUI kitchen-sink preview page. |

### Agent

- `mui-accessibility-auditor` — a read-heavy subagent for a thorough WCAG 2.2 AA + MUI-conformance audit without polluting the main context. Used by `/mui:audit`, or invoke directly.

### Hook

- A non-blocking `PostToolUse` hook (`hooks/check-frontend.sh`) that runs after edits to frontend files (`.html`, `.jsx`, `.tsx`, `.svelte`, `.vue`, `.astro`, `.mdx`). It flags a **high-confidence** subset of issues — missing `alt`, missing `lang`, positive `tabIndex`, click handlers on `<div>`/`<span>`, fake `href="#"` buttons, plus (in JSX/TSX) hand-rolled `<button>`/`<input>`/`<select>`/`<textarea>` where an MUI component exists — and surfaces them to Claude to fix. It never blocks and stays silent on clean files. It is a nudge, not a substitute for `/mui:audit`. Requires `jq`.

### Preview page

- `preview/index.html` — a **self-contained, static** kitchen-sink showcasing the core MUI components (app bar, buttons, text fields, selects, checkboxes/radios/switches, chips, cards, tabs, a data table, alerts, a dialog, progress, and a list) on the Material colour/type/elevation/spacing scales. It has **no build step** and opens directly from `file://` in any browser (inline CSS, no external fonts/scripts/CDNs). It is consumed by the create / describe-and-build flow, the review baseline, and the handover pack.

## Installing

Add the marketplace that lists this plugin, then install:

```bash
claude plugin marketplace add albitor-ltd/albitor-plugins
claude plugin install mui@albitor-plugins
```

Or load it directly for one session during development:

```bash
claude --plugin-dir /path/to/albitor-plugin-mui
```

Validate the plugin structure:

```bash
claude plugin validate /path/to/albitor-plugin-mui
```

## Keeping it current

The bundled references are a distilled snapshot, not a live mirror. MUI and Material Design change (the Grid API, the styling engine defaults, and theme options have shifted across MUI v4→v7), so the skills always tell the assistant to confirm exact, current detail against the live sources:

- Material UI (MUI) — <https://mui.com/>
- Material Design — <https://m3.material.io/>
- WCAG 2.2 — <https://www.w3.org/TR/WCAG22/>

To refresh the references, re-distil from those sources into `skills/*/references/` and bump the `version` in `.claude-plugin/plugin.json`.

## Licensing

- **Plugin code** (manifest, skills wiring, commands, agent, hook script, preview page): MIT — see [`LICENCE`](LICENCE).
- **Bundled reference content** under `skills/*/references/`: a distilled summary of the MUI documentation (MIT-licensed), Material Design (© Google), and WCAG 2.2 (© W3C, used under the W3C Document Licence). See [`NOTICE`](NOTICE) for attribution. Material UI, MUI, Material Design and Roboto are trademarks of their respective owners; this plugin is an independent aid and is not affiliated with or endorsed by MUI or Google.
