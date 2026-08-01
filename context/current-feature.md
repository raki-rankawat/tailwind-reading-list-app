# Current Feature

<!-- Feature Name -->

## Status

<!-- Not Started|In Progress|Completed -->

Completed

## Goals

<!-- Goals & requirements, copied from the feature spec -->

## Notes

<!-- Deviations from the design reference, arbitrary values used and why, anything left open -->

## History

<!-- Keep this updated. Earliest to latest. One line per completed screen-state. -->

- **0 — Setup.** Verified the Tailwind v4 CLI build and watcher, and defined 18 `@theme` colour tokens in `src/input.css` from the design reference's oklch values — status base/`-ink` pairs, home's light palette, search's dark palette and stripe bands, the shared accent, and star colours.
- **1 — Home structure & populated table.** Built `home.html` in four component stops (header → table frame + column headers → one complete row → the remaining five), adding one token (`--color-page-row-hover`) and five arbitrary type values; omitted the design's pagination row and its `cursor:pointer` on rows, and invented the hover/focus states the design lacks but the standards require. Also changed the workflow itself: components rather than layers, and an understanding note written one section per component as it lands.
