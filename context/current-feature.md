# Current Feature

<!-- Feature Name -->

1 — Home Page: Structure & Populated Table

## Status

<!-- Not Started|In Progress|Completed -->

In Progress

## Goals

<!-- Goals & requirements, copied from the feature spec -->

Start `home.html` from the top: page header, then the reading list table filled with real hardcoded sample data. This is the first real screen and the first real repetition of translating `Reading List.dc.html` into Tailwind classes.

- `home.html`: full `<html>`/`<head>`/`<body>`, linking `dist/output.css`
- Header section: "Reading List" title + an "Add Book" button (styled only — it goes nowhere)
- A real `<table>`: `<thead>` with Name/Type/Status/Score/Author/Link, `<tbody>` with 6 hardcoded `<tr>`s using the same sample titles/authors/statuses as your old `db.seed.json`
- Status shown as a colored pill using the tokens from feature 0 — never a raw hex value
- Score shown as filled/empty stars — inline SVGs or repeated characters, styled with Tailwind

Acceptance criteria: matches `Reading List.dc.html`'s populated table (column order, spacing, pill colors, star rendering); all 6 rows show correct status colors and star counts; no raw hex colors anywhere in this file's `class` attributes.

Built component by component, four stops: header → table frame + column headers → one complete body row → the remaining five rows.

## Notes

<!-- Deviations from the design reference, arbitrary values used and why, anything left open -->

## History

<!-- Keep this updated. Earliest to latest. One line per completed screen-state. -->

- **0 — Setup.** Verified the Tailwind v4 CLI build and watcher, and defined 18 `@theme` colour tokens in `src/input.css` from the design reference's oklch values — status base/`-ink` pairs, home's light palette, search's dark palette and stripe bands, the shared accent, and star colours.
