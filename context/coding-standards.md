# Coding Standards

## HTML

- Semantic elements over generic `div`s: real `<table>`/`<thead>`/`<tbody>`/`<tr>`/`<th>`/`<td>` for the table, real `<button>` for anything clickable-looking, real `<select>` for the status control, real `<dialog>` or a plain `<div role="dialog" aria-modal="true">` for the drawer and the delete popup
- One `<html>`/`<head>`/`<body>` per file — these are standalone pages, not fragments
- No inline `style="..."` attributes — if Tailwind's utilities genuinely can't express something, that's worth stopping on and asking about, not reaching for `style`
- Alt text on every real `<img>`; decorative images use `alt=""`

## Tailwind CSS

- Utility classes only — no authored CSS beyond `src/input.css`'s `@import "tailwindcss"` and its `@theme` block
- No component library (no shadcn/ui, no DaisyUI) — every visual is hand-built from utilities
- Custom colors (status colors, dark-palette tokens) go in `@theme` in `src/input.css`, named to match `project-overview.md` — never a raw hex value dropped into a `class` attribute
- Prefer Tailwind's default spacing/sizing scale (`p-4`, `gap-6`, `rounded-lg`) over arbitrary values (`p-[17px]`). Reach for an arbitrary value only when the design reference genuinely doesn't land on the scale, and note why in the feature's build notes
- Responsive variants (`md:`, `lg:`) are written mobile-first: base classes are the small-screen layout, breakpoint prefixes layer on top for larger viewports
- `hover:`, `focus:`, `focus-visible:` states are required on every interactive-looking element (buttons, the status select, card "Add" buttons) even though nothing behind them is wired up — a button that doesn't visually react to focus is a real accessibility gap, not a JS concern
- Group related utilities logically when a `class` list gets long (layout → spacing → typography → color → state) rather than in code order — for readability while you're still learning what each group does

## File Organization

- Exactly two output files: `home.html` and `search.html` — no shared partials, no templating
- The drawer is a **section stacked inside `home.html`**, below the populated table and in build order, not a separate file — see `project-overview.md`
- Separate stacked sections with a plain heading (e.g. `<h2>Drawer (shown open)</h2>`) so they're easy to tell apart while scrolling — this heading is a build aid, remove it only if asked to
- `src/input.css` — the only authored CSS file
- `dist/output.css` — generated, gitignored, linked from both HTML files' `<head>`
- No JS files. If you catch yourself creating one, stop and re-check the feature spec

## Naming

- Files: kebab-case, matching the screen-state name in `project-overview.md` (`home-populated.html`, not `HomePopulated.html` or `home_populated.html`)
- Custom Tailwind theme tokens: kebab-case, prefixed by what they're for (`--color-status-read`, `--color-search-bg`)

## Code Quality

- No commented-out markup left in a finished file
- No unused classes copy-pasted from a previous screen "just in case"
- Keep each file's `class` attributes honest to what's actually needed — if removing a class changes nothing visible, remove it before committing

## Explicitly Not Used

- No JavaScript of any kind, framework or vanilla
- No CSS preprocessor (Sass/Less) — Tailwind's `@theme` covers what you need
- No build tool beyond the Tailwind CLI (no Vite, no webpack, no bundler)
- No component library, no icon library beyond inline SVG copied by hand or a single small, named icon set if a feature spec calls for one
