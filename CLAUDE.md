# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A single-file [Quarto](https://quarto.org/) document (`index.qmd`) that renders an interactive horizontal timeline of Texas HCC (hepatocellular carcinoma) research — CPRIT grants and key publications from 2009–2025. The compiled output is `index.html` (self-contained, embed-resources: true).

## Build

```bash
quarto render index.qmd
```

Requires [Quarto CLI](https://quarto.org/docs/get-started/) installed. No R/Python packages needed — the document is pure HTML/CSS/JS embedded in Quarto raw blocks.

## Architecture

`index.qmd` contains everything in a single file:

1. **YAML front matter** — sets Quarto HTML format options (`flatly` theme, `embed-resources: true`, full-page layout).
2. **Intro section** (`{=html}` blocks) — stat cards and legend using plain CSS.
3. **Timeline CSS** (several `{=html}` style blocks) — layout for scroll container, axis, dots, connectors, rotated stub labels, and hover cards. Color-coded by event type: `grant` (navy), `pub` (crimson), `ramirez` (purple).
4. **Timeline JS** (final `{=html}` script block) — `EVENTS` array is the single source of truth. Renders dots/connectors/labels/cards into `#tl-inner` at runtime. Same-year events are x-jittered by 28px. Cards are hidden by default and revealed on `.tl-event:hover`. Drag-to-scroll and arrow buttons are wired here.

## Adding or Editing Events

All timeline data lives in the `EVENTS` array inside the `<script>` block (around line 222). Each entry:

```js
{ year: 2024, type: 'grant'|'pub'|'ramirez', emoji: '&#...',
  title: '...', journal: '...', inst: '...', pmid: '...', body: '...' }
```

`journal` and `pmid` are optional. `body` accepts inline HTML.

## Outputs

- `index.html` — self-contained rendered output (open directly in browser)
- `liver_timeline.pptx` — PowerPoint export (generated separately, not via Quarto)
- `timeline_full.png` — screenshot of the full timeline
- `_extensions/EmilHvitfeldt/` — Quarto extension (included for potential revealjs/style use)
