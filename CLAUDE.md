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

## Skill routing

When the user's request matches an available skill, invoke it via the Skill tool. The
skill has multi-step workflows, checklists, and quality gates that produce better
results than an ad-hoc answer. When in doubt, invoke the skill. A false positive is
cheaper than a false negative.

Key routing rules:

- Product ideas, "is this worth building", brainstorming → invoke /office-hours
- Strategy, scope, "think bigger", "what should we build" → invoke /plan-ceo-review
- Architecture, "does this design make sense" → invoke /plan-eng-review
- Design system, brand, "how should this look" → invoke /design-consultation
- Design review of a plan → invoke /plan-design-review
- Developer experience of a plan → invoke /plan-devex-review
- "Review everything", full review pipeline → invoke /autoplan
- Bugs, errors, "why is this broken", "wtf", "this doesn't work" → invoke /investigate
- Test the site, find bugs, "does this work" → invoke /qa (or /qa-only for report only)
- Code review, check the diff, "look at my changes" → invoke /review
- Visual polish, design audit, "this looks off" → invoke /design-review
- Developer experience audit, try onboarding → invoke /devex-review
- Ship, deploy, create a PR, "send it" → invoke /ship
- Merge + deploy + verify → invoke /land-and-deploy
- Configure deployment → invoke /setup-deploy
- Post-deploy monitoring → invoke /canary
- Update docs after shipping → invoke /document-release
- Weekly retro, "how'd we do" → invoke /retro
- Second opinion, codex review → invoke /codex
- Safety mode, careful mode, lock it down → invoke /careful or /guard
- Restrict edits to a directory → invoke /freeze or /unfreeze
- Upgrade gstack → invoke /gstack-upgrade
- Save progress, "save my work" → invoke /context-save
- Resume, restore, "where was I" → invoke /context-restore
- Security audit, OWASP, "is this secure" → invoke /cso
- Make a PDF, document, publication → invoke /make-pdf
- Launch real browser for QA → invoke /open-gstack-browser
- Import cookies for authenticated testing → invoke /setup-browser-cookies
- Performance regression, page speed, benchmarks → invoke /benchmark
- Review what gstack has learned → invoke /learn
- Tune question sensitivity → invoke /plan-tune
- Code quality dashboard → invoke /health
