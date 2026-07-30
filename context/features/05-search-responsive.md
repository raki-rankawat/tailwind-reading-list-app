# Feature: Search Page — Responsive

## Status

Not Started

## Goal

Make the card grid and header in `search.html` respond correctly at mobile widths — the last feature in the plan.

## Requirements

- Card grid reflows to fewer columns below typical mobile breakpoints, using `md:`/`lg:` grid utilities on the existing markup
- Header/search input stacks or resizes sensibly at narrow widths

## Acceptance Criteria

- Resizing an actual browser window shows a sensible column count at mobile, tablet, and desktop widths

## Depends On

- Feature 4 (grid)

## Notes

Once this is done, the plan is complete: two files, both screens, no leftover scaffolding. Do one last pass over both files checking that every color traces back to a token from feature 0 and no raw hex value snuck in anywhere.
