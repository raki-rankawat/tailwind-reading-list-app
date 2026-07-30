# Reading List — Tailwind Template

A learn-by-doing companion to the working Reading List app: its two real pages, rebuilt in plain HTML + Tailwind CSS, no framework, no JavaScript. See `CLAUDE.md` and `context/` for the full plan.

Two files, built top to bottom: `home.html` (table + drawer) and `search.html` (dark card grid).

## Progress

| # | Feature | File | Status |
|---|---------|------|--------|
| 0 | Setup — Tailwind CLI + core tokens | — | Completed |
| 1 | Home — structure & populated table | `home.html` | Not Started |
| 2 | Home — drawer, status control, delete popup | `home.html` | Not Started |
| 3 | Home — responsive | `home.html` | Not Started |
| 4 | Search — structure & populated grid | `search.html` | Not Started |
| 5 | Search — responsive | `search.html` | Not Started |

## Build Log

<!-- One entry per completed feature, earliest to latest. -->

- **0 — Setup.** Tailwind v4.3.3 CLI build verified (`build` one-shot, `watch` rebuilds ~1s on save) and an 18-token `@theme` block added to `src/input.css`. Tokens are taken from `Reading List.dc.html`'s oklch values, not `project-overview.md`'s hexes — the design's status pills are a pale tint plus a darker same-hue ink, so all six status tokens (base + `-ink`) exist from the start rather than waiting for a contrast failure. Also defined ahead of the spec's literal list, to avoid stopping mid-page later: home's light palette (`--color-page-*`), search's dark palette (`--color-search-*`), the shared `--color-accent`, and star fill/empty colors. No `tailwind.config.js` — v4 replaces it with `@theme`. The cover-placeholder stripe is stored as two colour tokens rather than one, since the gradient itself is a background-image; feature 4 composes them via an arbitrary value that references the vars. Deferred to feature 2: the Delete button's danger pair and the light stripe pair.
