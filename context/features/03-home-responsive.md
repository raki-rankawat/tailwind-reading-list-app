# Feature: Home Page — Responsive

## Status

Not Started

## Goal

Make everything already in `home.html` — table and drawer — respond correctly at mobile widths, using `md:`/`lg:` utility prefixes directly on the existing markup rather than duplicate sections.

## Requirements

- Table: below 768px, show Name + Status columns only, with the author moved under the title — **hide columns with Tailwind's `hidden`/`md:table-cell`, don't `display: block` the table** (breaks table semantics — see your old build's accessibility notes on this exact mistake)
- Drawer: below 768px, becomes a full-screen overlay instead of a 400px side panel
- Header: stacks instead of overflowing at narrow widths

## Acceptance Criteria

- Resizing an actual browser window shows the mobile layout below 768px and the desktop layout at ≥1024px for both the table and the drawer
- No `display: block` applied to any table element at any breakpoint

## Depends On

- Feature 1 (table), Feature 2 (drawer)

## Design Reference

`Reading List.dc.html` likely only shows desktop — build mobile against @context/project-overview.md's responsive table, same as your original build.

## Notes

This closes out `home.html`. Everything after this feature works on `search.html`.
