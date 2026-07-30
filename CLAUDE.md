# Reading List — Tailwind Template (Learning Project)

A **frontend-only, design-only** rebuild of the Reading List app: plain HTML + Tailwind CSS, no framework, no JavaScript logic, no backend. The goal is not the app — you already built and shipped the real thing. The goal is learning Tailwind by re-producing its two pages by hand, top to bottom, one chunk at a time.

Two output files only: `home.html` (table + drawer, since the drawer opens over the home page) and `search.html`. The drawer is a stacked section inside `home.html`, not a separate file — see @context/project-overview.md.

## Context Files

Read the following to get the full context of the project:

- @context/project-overview.md
- @context/coding-standards.md
- @context/ai-interaction.md
- @context/current-feature.md

## Commands

```bash
npm install         # installs tailwindcss + the CLI, nothing else
npm run build       # one-shot build: reads src/input.css -> writes dist/output.css
npm run watch       # rebuilds output.css on every save while you work
```

There is no dev server. Open the relevant `.html` file directly in a browser (or use a static file server / VS Code Live Server if you want, but it's not required — these pages have zero JavaScript and zero fetch calls, so `file://` works fine).

## What's explicitly NOT here

- No React, no Next.js, no TypeScript
- No json-server, no `db.json`, no data fetching
- No interactivity — the drawer, status control, and delete popup are static markup shown already open, not something a click toggles
- No build tooling beyond the Tailwind CLI itself
- No file sprawl — if you find yourself about to create a third `.html` file, stop; it belongs as a section inside `home.html` or `search.html` instead

If you find yourself reaching for `<script>` and an event listener to make something "actually work," stop — that's a sign the feature belongs in a future, separate project, not this one.
