# Feature: Home Page — Structure & Populated Table

## Status

Not Started

## Goal

Start `home.html` from the top: page header, then the reading list table filled with real hardcoded sample data. This is the first real screen and the first real repetition of translating `Reading List.dc.html` into Tailwind classes.

## Requirements

- `home.html`: full `<html>`/`<head>`/`<body>`, linking `dist/output.css`
- Header section: "Reading List" title + an "Add Book" button (styled only — it goes nowhere)
- A real `<table>`: `<thead>` with Name/Type/Status/Score/Author/Link, `<tbody>` with 6 hardcoded `<tr>`s using the same sample titles/authors/statuses as your old `db.seed.json`
- Status shown as a colored pill using the tokens from feature 0 — never a raw hex value
- Score shown as filled/empty stars — inline SVGs or repeated characters, styled with Tailwind

## Acceptance Criteria

- Matches `Reading List.dc.html`'s populated table: column order, spacing, pill colors, star rendering
- All 6 rows show correct status colors and star counts
- No raw hex colors anywhere in this file's `class` attributes

## Depends On

- Feature 0 (tokens, build pipeline)

## Design Reference

Import via claude-design MCP: https://claude.ai/design/p/529c3d37-dc74-4ef9-8d5a-e758ce3e5835?file=Reading+List.dc.html

Read `Reading List.dc.html`'s table section fresh. Don't port your old `BookTable.tsx` JSX with the React stripped out — re-deriving the classes from the visual reference is the actual exercise.

## Notes

The drawer is the next feature, added directly below this table in the same file — it overlays a second copy of this markup, so get the table right here rather than fixing it twice.
