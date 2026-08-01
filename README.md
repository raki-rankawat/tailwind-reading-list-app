# Reading List — Tailwind Template

A learn-by-doing companion to the working Reading List app: its two real pages, rebuilt in plain HTML + Tailwind CSS, no framework, no JavaScript. See `CLAUDE.md` and `context/` for the full plan.

Two files, built top to bottom: `home.html` (table + drawer) and `search.html` (dark card grid).

## Progress

| # | Feature | File | Status |
|---|---------|------|--------|
| 0 | Setup — Tailwind CLI + core tokens | — | Completed |
| 1 | Home — structure & populated table | `home.html` | Completed |
| 2 | Home — drawer, status control, delete popup | `home.html` | Not Started |
| 3 | Home — responsive | `home.html` | Not Started |
| 4 | Search — structure & populated grid | `search.html` | Not Started |
| 5 | Search — responsive | `search.html` | Not Started |

## Build Log

<!-- One entry per completed feature, earliest to latest. -->

- **0 — Setup.** Tailwind v4.3.3 CLI build verified (`build` one-shot, `watch` rebuilds ~1s on save) and an 18-token `@theme` block added to `src/input.css`. Tokens are taken from `Reading List.dc.html`'s oklch values, not `project-overview.md`'s hexes — the design's status pills are a pale tint plus a darker same-hue ink, so all six status tokens (base + `-ink`) exist from the start rather than waiting for a contrast failure. Also defined ahead of the spec's literal list, to avoid stopping mid-page later: home's light palette (`--color-page-*`), search's dark palette (`--color-search-*`), the shared `--color-accent`, and star fill/empty colors. No `tailwind.config.js` — v4 replaces it with `@theme`. The cover-placeholder stripe is stored as two colour tokens rather than one, since the gradient itself is a background-image; feature 4 composes them via an arbitrary value that references the vars. Deferred to feature 2: the Delete button's danger pair and the light stripe pair.
- **1 — Home structure & populated table.** `home.html` built in four component stops (header → table frame + column headers → one complete row → the remaining five), which replaced the original layer-by-layer pacing after the first attempt made every review span the whole page. Feature 0's tokens covered everything except the row hover, so one token was added: `--color-page-row-hover: oklch(0.98 0.003 90)`, sitting between the card's white and the page's `page-bg` so a hovered row lifts without reading as selected. Five arbitrary values, all because the design was authored off Tailwind's type scale: `text-[28px]` and the half-pixel sizes `text-[11.5px]`/`text-[13.5px]`/`text-[14.5px]` (nothing exists between `text-xs` and `text-sm`), plus `tracking-[-0.01em]` (`tracking-tight` is `-0.025em`, more than twice as tight). The page width was initially a sixth, written `max-w-[1100px]` because `max-w-5xl`/`6xl` straddle it at 1024/1152 — the Tailwind IDE extension caught that the numeric spacing scale also covers `max-w`, so `max-w-275` (275 × 4px) hits 1100px exactly and the arbitrary value was wrong. Worth remembering: divide the design's value by 4 before reaching for brackets. Everything else landed exactly on the scale — all six spacing values, `rounded-lg`/`rounded-xl`, and `tracking-wider` hitting the design's `.05em` on the nose. `shadow-xs` substitutes for the design's `0 1px 2px rgba(0,0,0,0.04)`, an alpha hundredth apart. Three deviations from the design reference: the pagination row is omitted (the design pages at 5, the spec asks for 6 rows in one table and never mentions pagination), `cursor:pointer` is dropped from rows because no click is wired and a pointer would promise one, and hover/focus states on the Add Book button and the row links are inventions — the design specifies none, the coding standards require them. Also changed the workflow itself: understanding notes are now mandatory and written one section per component as it lands, so `context/understanding/01-home-structure-table.md` documents every class alongside the build rather than after it.
