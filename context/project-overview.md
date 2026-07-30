# Reading List — Tailwind Template

> Re-building the visual layer of the Reading List app in plain HTML + Tailwind CSS, screen state by screen state, to learn Tailwind by doing.

---

## Table of Contents

- [Purpose](#purpose)
- [What This Is Not](#what-this-is-not)
- [Screens & States](#screens--states)
- [Tech Stack](#tech-stack)
- [File Structure](#file-structure)
- [Design Reference](#design-reference)
- [Status Colors](#status-colors)
- [Responsive Behavior](#responsive-behavior)
- [Explicit Non-Goals](#explicit-non-goals)

---

## Purpose

You already built the working Reading List app (Next.js + TypeScript + json-server). This project throws that away and rebuilds **only the markup and styling**, using plain HTML and Tailwind CSS utility classes, with the working app's design as the reference to copy. The point is repetition with a real target: you're not inventing a design, you're translating a known design into Tailwind classes with your own hands, enough times that the utility-class vocabulary stops requiring lookup.

## What This Is Not

- Not a rebuild of the app's functionality. Nothing here fetches, saves, or reacts to a click.
- Not a component library exercise. Every screen state is a **standalone HTML file** — there is no shared header/footer include system, no templating. Some duplication across files is expected and fine; the repetition is part of what teaches you the classes.
- Not going to end in something you can plug a backend into later without rewriting it. If that's the eventual goal, that's a different, later project.

## Screens & States

Only two output files, matching the app's two real pages. The drawer isn't a third file — it's a **stacked section** within `home.html`, since it opens on top of the home page rather than navigating away from it:

| File | What it contains, top to bottom |
|---|---|
| `home.html` | Header → populated table (6 hardcoded rows, status pills, star scores) → drawer, shown open and overlaid on a second copy of the table, including the status control and the delete-confirmation popup → responsive rules for all of the above |
| `search.html` | Header + search input → populated dark card grid (~8-12 hardcoded results, mixing `Add`/`Adding…`/`Already Added` button states across a few cards) → responsive rules for all of the above |

Stacking the drawer in the same file means scrolling down `home.html` shows the bare populated table, then directly below it that same table with the drawer over it — easy to compare without switching files. A short `<h2>Drawer (shown open)</h2>`-style label above the section is fine and expected — it's a build aid, not a design requirement.

## Tech Stack

| Category | Technology | Notes |
|---|---|---|
| **Markup** | Plain HTML | No templating engine, no JSX |
| **Styling** | Tailwind CSS (CLI, not CDN) | `npx tailwindcss` compiling `src/input.css` → `dist/output.css` |
| **Config** | `tailwind.config.js` + `@theme` custom properties | For the status colors and dark-palette tokens — see feature 0 |
| **JavaScript** | None | Zero `<script>` tags with logic in them. If a design detail needs JS to actually function (the drawer sliding open, the dropdown opening), you build the **end state** it lands on, not the mechanism |

## File Structure

```
reading-list-tailwind-template/
├── src/
│   └── input.css          # @import "tailwindcss"; + @theme customizations (tokens live
│                          #   here, defined inline as feature 0 needs them — no swatch page)
├── dist/
│   └── output.css         # generated — gitignored
├── home.html              # header, populated table, drawer, delete popup
└── search.html            # header, search input, dark card grid, button states
```

## Design Reference

Same source as the original build: `https://claude.ai/design/p/529c3d37-dc74-4ef9-8d5a-e758ce3e5835?file=Reading+List.dc.html`

Key file: `Reading List.dc.html` (also reads `support.js`)

This is the source of truth for every screen — layout, element order, sizes, colors, spacing. Read it **before** building each screen, not after, and match it. It's a design reference, not implementation code — its inline styles are what you're translating into Tailwind utilities, not copying verbatim.

You've already built this once as working React components — resist the urge to just port your old JSX and strip the logic out. Read the design file fresh each time and re-derive the Tailwind classes yourself. Porting old code teaches you nothing; re-deriving the classes is the entire point of this project.

Deviate only where a feature spec explicitly says to, and record the deviation in `context/current-feature.md`.

## Status Colors

```css
:root {
  --color-status-read: #22c55e;             /* Green */
  --color-status-currently-reading: #f97316; /* Orange */
  --color-status-want-to-read: #3b82f6;      /* Blue */
}
```

Your original build ended up needing six tokens (base + a darker `-ink` variant per status) once you tried to hit real contrast for text on a tinted pill — see feature 0, and decide again for yourself whether you hit the same wall.

## Responsive Behavior

| Viewport | Table | Drawer |
|---|---|---|
| Desktop (≥1024px) | Full table | Slide-in from right (built here as a static "already open" state) |
| Mobile (<768px) | Simplified rows (Name + Status, author under title) | Full-screen overlay |

## Explicit Non-Goals

- No JavaScript, no interactivity, no state
- No component reuse system (no partials/includes)
- No backend, no data fetching, no persistence
- No accessibility-via-ARIA-state (e.g. `aria-expanded`) since there's no toggle to describe — static, structurally-sound HTML and correct semantics (real `<table>`, real `<button>`, real `<select>`) still apply and are not optional
- No animation/transition logic beyond what a `transition` utility class does on `:hover`/`:focus` (those are fine — they're pure CSS, not "implementation")

---

_This document describes the Tailwind template project only. It is a companion learning exercise to the working app, not a replacement for it or a step toward one._
