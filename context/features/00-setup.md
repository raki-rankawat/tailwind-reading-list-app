# Feature: Setup — Tailwind CLI + Core Tokens

## Status

Not Started

## Goal

Get a working Tailwind build, and define the handful of custom color tokens you already know you'll need, before either page exists. No swatch page — just get the pipeline and the tokens right so features 1 and beyond don't stop to add a token mid-page.

## Requirements

- `npm init -y`, install `tailwindcss` + `@tailwindcss/cli` as dev dependencies
- `src/input.css`: `@import "tailwindcss";` plus an `@theme` block defining:
  - `--color-status-read`, `--color-status-currently-reading`, `--color-status-want-to-read` (values in @context/project-overview.md)
  - Whatever dark-palette background/card tokens `search.html` will need — check `Reading List.dc.html`'s search section for the actual values now, so they exist before feature 4
- `package.json` scripts: `"build"` (one-shot) and `"watch"` (rebuild on save)
- `.gitignore` covers `node_modules/` and `dist/`
- A throwaway one-line `<h1 class="text-3xl font-bold text-blue-600">it works</h1>` in a scratch file (not `home.html`/`search.html`) just to prove the build pipeline is live, then delete it

## Acceptance Criteria

- `npm run watch` rebuilds `dist/output.css` on save
- Both future files can link `dist/output.css` and immediately use the custom color tokens

## Depends On

- Nothing — this is first

## Notes

If you hit the same problem your old build did — a single status color failing contrast when used as text on its own tinted background — that's the moment to add the darker `-ink` variant tokens, not before. Don't pre-add them speculatively.
