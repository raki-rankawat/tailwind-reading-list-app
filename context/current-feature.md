# Current Feature

Feature 0 — Setup: Tailwind CLI + Core Tokens

## Status

In Progress

## Goals

Get a working Tailwind build, and define the handful of custom color tokens already known to be
needed, before either page exists. No swatch page — just get the pipeline and the tokens right so
features 1 and beyond don't stop to add a token mid-page.

Requirements (from `context/features/00-setup.md`):

- `npm init -y`, install `tailwindcss` + `@tailwindcss/cli` as dev dependencies
- `src/input.css`: `@import "tailwindcss";` plus an `@theme` block defining:
  - `--color-status-read`, `--color-status-currently-reading`, `--color-status-want-to-read`
    (values in `context/project-overview.md`)
  - Whatever dark-palette background/card tokens `search.html` will need — taken from
    `Reading List.dc.html`'s search section now, so they exist before feature 4
- `package.json` scripts: `"build"` (one-shot) and `"watch"` (rebuild on save)
- `.gitignore` covers `node_modules/` and `dist/`
- A throwaway one-line `<h1 class="text-3xl font-bold text-blue-600">it works</h1>` in a scratch
  file (not `home.html`/`search.html`) just to prove the build pipeline is live, then delete it

Acceptance criteria:

- `npm run watch` rebuilds `dist/output.css` on save
- Both future files can link `dist/output.css` and immediately use the custom color tokens

## Notes

**Deviation — status colour values.** `context/project-overview.md` documents the status colours as
`#22c55e` / `#f97316` / `#3b82f6`. The design reference uses neither those hexes nor a single colour
per status: each pill is a pale tinted background plus a much darker same-hue ink for its label
(`PILL` map in `Reading List.dc.html`). Taking the design as source of truth per
`project-overview.md`, so six tokens are defined, base + `-ink`, in the design's oklch values.
`project-overview.md`'s Status Colors section is now stale.

**Deviation — no `tailwind.config.js`.** `project-overview.md`'s Tech Stack row says
"`tailwind.config.js` + `@theme`". Installed Tailwind is v4.3.3, where `@theme` in `src/input.css`
replaces the config file entirely. No config file exists or will.

**Beyond the spec's literal requirement list.** The spec names only the status and dark-palette
tokens, but its goal is that features 1+ never stop to add a token mid-page. So also defined:
home's light palette (`--color-page-*`), the shared `--color-accent`, and the star colours.

**Deliberately left out** — both belong to feature 2's drawer, not here: the Delete Book button's
danger pair (`oklch(0.75 0.14 25)` border, `oklch(0.5 0.16 25)` text) and the light cover-placeholder
stripe pair (`oklch(0.92 0.005 90)` / `oklch(0.95 0.003 90)`).

## History

<!-- Keep this updated. Earliest to latest. One line per completed screen-state. -->
