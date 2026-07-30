# Feature: Search Page — Structure & Populated Grid

## Status

Not Started

## Goal

Start `search.html` from the top: dark-themed header and search input, then the populated card grid with hardcoded sample results — including a realistic mix of button states on the cards, so you don't need a separate file just to compare `Add`/`Adding…`/`Already Added`.

## Requirements

- `search.html`: full `<html>`/`<head>`/`<body>`, dark background using feature 0's dark tokens
- Header: title + a search input (styled, not functional)
- A responsive card grid, ~8-12 hardcoded cards: cover image, title, author, star rating
- Vary the button across a few cards rather than making all of them identical:
  - Most cards: default `Add` button
  - One card: disabled `Adding…` (muted, `disabled` attribute set)
  - One card: `Already Added` — a muted **label**, not a disabled button (per your old build, these are intentionally different elements)
- Give each `Add` button a distinguishing `aria-label` (e.g. `Add [Title] to your reading list`)

## Acceptance Criteria

- Matches `Reading List.dc.html`'s search view: dark palette, spacing, card shape, grid columns
- Grid reflows sensibly on a normal browser resize, no fixed pixel widths breaking it
- The three button states are each represented and visually distinguishable

## Depends On

- Feature 0 (dark tokens)

## Design Reference

`Reading List.dc.html`'s search/card-grid section. Note the grid column count and gap specifically — usually the first thing that's slightly wrong on a naive rebuild.
