# 01 — Home: Structure & Populated Table

A walkthrough of `home.html`, written one component at a time as each one is built. Assumes you've read `00-setup.md` — this note takes the tokens defined there and finally spends them.

**Spec:** `context/features/01-home-structure-table.md` · **Design:** `Reading List.dc.html`

---

## Stop 1 — The page shell and the header

### What's on screen

```
  ┌─────────────────────────────────────────────────────────────────┐
  │                        warm off-white page                      │  ← <body>
  │                                                                 │
  │      ┌───────────────────────────────────────────────────┐      │
  │      │  My Reading List                  ┌─────────────┐ │      │
  │      │  6 books                          │ + Add Book  │ │      │  ← <header>
  │      │                                   └─────────────┘ │      │
  │      └───────────────────────────────────────────────────┘      │
  │      ◄──────────────── max 1100px ──────────────────────►       │
  └─────────────────────────────────────────────────────────────────┘
```

### The markup, stripped to its skeleton

```
<body class="min-h-screen bg-page-bg">
 └─ <main class="mx-auto max-w-[1100px] pt-14 px-8 pb-20">
     └─ <header class="flex items-center justify-between mb-8">
         ├─ <div>                                    ← shoved LEFT  ─┐
         │   ├─ <h1>My Reading List</h1>                             │  all the leftover
         │   └─ <p>6 books</p>                                       │  space goes in
         └─ <button>+ Add Book</button>              ← shoved RIGHT ─┘  the middle
```

### Every class, one by one

**On `<body>`**

| Class | The CSS it generates | Why this one |
|---|---|---|
| `min-h-screen` | `min-height: 100vh` | The design's outer wrapper is `min-height:100vh`. **Min**-height, not `height`, so a long page can still grow past one screen. With `h-screen` the table would eventually overflow instead of pushing the page taller |
| `bg-page-bg` | `background-color: var(--color-page-bg)` | Your token from feature 0, `oklch(0.985 0.003 90)`. Not `bg-white` — the design's page is very slightly warm, and the white table card sitting on it later is what makes that difference visible |

**On `<main>`**

| Class | The CSS it generates | Why this one |
|---|---|---|
| `mx-auto` | `margin-inline: auto` | Centres the column. See call-out 3 — this does nothing without a width limit next to it |
| `max-w-[1100px]` | `max-width: 1100px` | The design says `max-width:1100px`. Square brackets because Tailwind has no name for 1100 — see call-out 2 |
| `pt-14` | `padding-top: calc(var(--spacing) * 14)` = **56px** | Design: `padding: 56px 32px 80px`. See call-out 1 for where 14 → 56 comes from |
| `px-8` | `padding-inline: calc(var(--spacing) * 8)` = **32px** | The `x` means left *and* right at once. `inline` rather than `left`/`right` — call-out 6 |
| `pb-20` | `padding-bottom: calc(var(--spacing) * 20)` = **80px** | Bottom padding is bigger than top on purpose: it stops the last table row from sitting flush against the bottom of the window |

**On `<header>`**

| Class | The CSS it generates | Why this one |
|---|---|---|
| `flex` | `display: flex` | Turns the two children into a row. Without it they'd stack vertically, since `<div>` and `<button>` are block-level |
| `items-center` | `align-items: center` | Vertically centres the button against the two-line title block. Drop it and the button sticks to the top |
| `justify-between` | `justify-content: space-between` | First child hard left, last child hard right, all leftover space dumped in the gap. This is why no margin is needed on the button |
| `mb-8` | `margin-bottom: calc(var(--spacing) * 8)` = **32px** | Design: `margin-bottom:32px`. The gap between the header and the table that arrives in stop 2 |

**On `<h1>` and `<p>`**

| Class | The CSS it generates | Why this one |
|---|---|---|
| `text-[28px]` | `font-size: 28px` | Design: `font-size:28px`. Tailwind's scale jumps `text-2xl` (24px) → `text-3xl` (30px), straddling it |
| `font-semibold` | `font-weight: 600` | Design: `font-weight:600`. `font-bold` would be 700 — visibly heavier |
| `tracking-[-0.01em]` | `letter-spacing: -0.01em` | Design: `letter-spacing:-0.01em`. The named `tracking-tight` is `-0.025em`, **more than twice as tight** — this is a real difference at 28px, not a rounding argument |
| `text-page-ink` | `color: var(--color-page-ink)` | Your near-black token. Not `text-black`; the design's ink is a soft `oklch(0.22 0.01 90)` |
| `mt-1.5` | `margin-top: calc(var(--spacing) * 1.5)` = **6px** | Design: `margin:6px 0 0`. The scale takes fractions — `1.5 × 4px` |
| `text-sm` | `font-size: var(--text-sm)` = **14px**, plus a line-height | Design: `font-size:14px`. Landed exactly on the named scale, so no brackets needed |
| `text-page-muted` | `color: var(--color-page-muted)` | The grey token. Same hue 90 as the ink, just lighter — that's the `oklch` trick from `00-setup.md` |

**On `<button>`**

| Class | The CSS it generates | Why this one |
|---|---|---|
| `rounded-lg` | `border-radius: var(--radius-lg)` = **8px** | Design: `border-radius:8px`. Exact match to the named value |
| `px-5` / `py-2.5` | `padding-inline: 20px` / `padding-block: 10px` | Design: `padding:10px 20px`. Note the order flips: CSS shorthand is vertical-then-horizontal, Tailwind names each axis explicitly |
| `text-sm` `font-semibold` | `14px` / `600` | Same as above |
| `bg-accent` | `background-color: var(--color-accent)` | `#111827`, the one token shared by both pages |
| `text-white` | `color: var(--color-white)` = `#fff` | Built-in, not one of your tokens — the standards ban raw hex in markup, and `text-white` is a name, not a hex |
| `transition-opacity` | `transition-property: opacity` + 150ms default | Without it the hover would snap instantly. See call-out 4 |
| `hover:opacity-90` | `opacity: 90%` on hover | See call-outs 4 and 5 — including a surprise about when this does *not* apply |
| `focus-visible:outline-2` | `outline-width: 2px` on focus-visible | See call-out 5 |
| `focus-visible:outline-offset-2` | `outline-offset: 2px` | Pushes the ring 2px clear of the button so it reads as a ring, not a border |
| `focus-visible:outline-accent` | `outline-color: var(--color-accent)` | Same token as the background, so the ring is unmistakably part of this button |

---

### Call-out 1 — Where does `pt-14` get 56px from?

This is the single most useful thing to internalise about Tailwind. There is **one number** behind the whole spacing system:

```
  --spacing: 0.25rem      which is 4px
```

Every spacing class is just that number multiplied:

```
      pt-14   →   14 × 4px   =   56px
      px-8    →    8 × 4px   =   32px
      pb-20   →   20 × 4px   =   80px
      mb-8    →    8 × 4px   =   32px
      mt-1.5  →  1.5 × 4px   =    6px
      py-2.5  →  2.5 × 4px   =   10px
```

So **to convert a design's pixel value into a Tailwind class, divide by 4.** The design said 56px; 56 ÷ 4 = 14; the class is `pt-14`.

```
   design says 56px  ──►  ÷ 4  ──►  14  ──►  pt-14   ✔ lands on the scale
   design says 17px  ──►  ÷ 4  ──►  4.25 ──► ✘ not a step — needs p-[17px]
```

Every single spacing value in this header divided cleanly. That is not luck — the design was drawn on a 4px grid, which is the same grid Tailwind assumes.

### Call-out 2 — What the square brackets mean

`max-w-[1100px]`, `text-[28px]` and `tracking-[-0.01em]` all have brackets. They're saying: **"Tailwind, you don't have a name for this. Use this literal value."**

```
      max-w-2xl        max-w-[1100px]
      └──┬──┘          └──┬──┘└──┬──┘
      a NAME Tailwind    the      the exact value
      already knows      family   you're forcing in
```

Why these three needed it:

```
   WANTED            NEAREST NAMES            VERDICT
   1100px      max-w-5xl=1024  6xl=1152   ✘ straddles — 76px out either way
   28px        text-2xl=24     3xl=30     ✘ straddles
   -0.01em     tracking-tight = -0.025em  ✘ nearest name is 2.5× too tight
   56px        pt-14 = 56px               ✔ exact — no brackets
```

The coding standards say reach for brackets **only** when the design genuinely misses the scale, and to write down why. That's what the table above is doing. A file full of `p-[17px]` means you stopped using the design system.

### Call-out 3 — How `mx-auto` actually centres

A very common beginner trap: `mx-auto` **on its own does nothing.**

```
   ✘ WITHOUT a width limit              ✔ WITH max-w-[1100px]

   ┌──────────────────────────┐         ┌──────────────────────────┐
   │████████████████████████████│        │      ████████████        │
   │  block fills 100% width   │         │   auto  auto             │
   │  "auto" margins = 0       │         │   splits the leftover    │
   └──────────────────────────┘         └──────────────────────────┘
```

`margin-inline: auto` means "share the leftover space equally on both sides". If the element is already as wide as its parent there **is** no leftover space, so nothing moves. The pair `mx-auto` + `max-w-*` is what centres — always both, never one.

### Call-out 4 — How to read a `hover:` prefix

```
       hover:opacity-90
       └─┬─┘ └────┬────┘
         │        └──── what to apply
         └───────────── when to apply it
```

It compiles to `.hover\:opacity-90:hover { opacity: 90% }`. The class sits in the markup permanently; the browser only *honours* it while the pointer is over the element.

`transition-opacity` is the separate half that makes it fade rather than snap:

```
   without transition-opacity      with transition-opacity
   ────────────────────────────    ────────────────────────────
   100% ──────────┐                100% ─────╲
                  │                            ╲___ over 150ms
    90%           └──────           90%            ────────────
        instant, twitchy                smooth
```

**Surprise worth knowing.** Tailwind wraps hover in a media query:

```css
@media (hover: hover) {
  .hover\:opacity-90:hover { opacity: 90%; }
}
```

That means **hover styles are switched off on touch devices.** Without it, tapping a button on a phone leaves it stuck in its hover state until you tap elsewhere. You get this protection for free — but it's also why hover must never be the *only* way something is visible.

### Call-out 5 — Why `focus-visible:` and not `focus:`

They sound identical and behave very differently.

```
                          focus:               focus-visible:
   click with a mouse  →  ring appears  ✘      no ring        ✔
   tab with keyboard   →  ring appears  ✔      ring appears   ✔
```

`focus:` fires on *any* focus, including a plain mouse click — which is why so many sites look like they have a stray blue box after you click a button. `focus-visible:` lets the browser decide, and browsers only show it when the user is navigating by keyboard, which is exactly who needs it.

The three classes build one ring together:

```
   ┌ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┐   ← outline-2         2px thick
     ┌───────────────┐     ← outline-offset-2  2px gap, so it reads as a ring
     │  + Add Book   │
     └───────────────┘
   └ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┘   ← outline-accent    coloured with the button's own token
```

**This hover and focus pair is not in the design file.** The design gives the button only `cursor:pointer` — no hover, no focus. Your `coding-standards.md` requires both on anything interactive, so this is a deliberate, recorded addition rather than a copy of the reference.

### Call-out 6 — `padding-inline`, not `padding-left`

`px-8` generates `padding-inline: 32px`, not `padding-left` + `padding-right`. These are **logical properties**: they follow the direction the text runs, so in a right-to-left language they'd flip automatically. For this English-only page the effect is identical — but it explains why the generated CSS doesn't say what you might expect.

```
   px-8  →  padding-inline  →  the "start" and "end" edges of the line
   py-4  →  padding-block   →  the edges across the lines (top/bottom)
```

---

### Three things deliberately *not* written

An absence is invisible in the markup, so it's worth naming what was left out on purpose.

| The design says | No class written | Why |
|---|---|---|
| `margin:0` on the `<h1>` and `<p>` | — | Tailwind's preflight already zeroes margins on every element (`dist/output.css` line 42). Writing `m-0` would be a no-op |
| `display:flex; align-items:center; gap:6px` on the button | — | The button has exactly one text child, so all three do nothing visible. The standards say remove any class whose removal changes nothing |
| `cursor:pointer` on the button | — | It's a real `<button>`, and browsers already show the pointer cursor for buttons |

The lesson generalises: **before translating a declaration from the design, check whether preflight or the element's own semantics already give it to you.** Using a real `<button>` instead of a styled `<div>` deleted work here.

---

## Stop 2 — The table frame and its column headers

### What's on screen

```
      ┌───────────────────────────────────────────────────────────┐  ← rounded-xl (12px)
      │  NAME    TYPE    STATUS    SCORE    AUTHOR    LINK        │     border-page-line
      ├───────────────────────────────────────────────────────────┤  ← border-b on the <tr>
      │                                                           │
      │            <tbody> — empty until stop 3                   │     bg-white
      │                                                           │     shadow-xs
      └───────────────────────────────────────────────────────────┘
```

Right now this renders as a white strip with a hairline rule under the column names. It looks unfinished because it *is* — the rows arrive next.

### The markup, stripped to its skeleton

```
<div class="overflow-hidden rounded-xl border border-page-line bg-white shadow-xs">
 └─ <table class="w-full">                    ← the CARD is the div, not the table
     ├─ <thead>
     │   └─ <tr class="border-b border-page-line">
     │       └─ <th scope="col"> × 6          ← Name Type Status Score Author Link
     └─ <tbody></tbody>                       ← waiting for stop 3
```

Notice the **card and the table are two different elements.** The rounded corners, border and shadow live on a wrapping `<div>`; the `<table>` inside stays plain. That split is what makes the next call-out work.

### Every class, one by one

**On the card `<div>`**

| Class | The CSS it generates | Why this one |
|---|---|---|
| `overflow-hidden` | `overflow: hidden` | Not cosmetic — it's what makes `rounded-xl` actually clip its contents. See call-out 7 |
| `rounded-xl` | `border-radius: var(--radius-xl)` = **12px** | Design: `border-radius:12px`. Exact name match, no brackets needed. Note `rounded-lg` on the button was 8px — the card is deliberately rounder than the button |
| `border` | `border-width: 1px` | Width only. The colour is a separate class — see call-out 8 |
| `border-page-line` | `border-color: var(--color-page-line)` | Your hairline token. Colour only |
| `bg-white` | `background-color: #fff` | Design: `background:#fff`. Literally white, unlike the page behind it which is the warm `bg-page-bg`. That contrast is the whole reason the card reads as a card |
| `shadow-xs` | `0 1px 2px 0 rgb(0 0 0 / 0.05)` | Design: `0 1px 2px rgba(0,0,0,0.04)`. A hundredth of an alpha apart at 2px blur — not a visible difference, so the named utility beat an arbitrary one |

**On `<table>`**

| Class | The CSS it generates | Why this one |
|---|---|---|
| `w-full` | `width: 100%` | Design: `width:100%`. Without it a table shrinks to fit its content, leaving a gap on the right of the card. Tables size to content by default — unlike a `<div>`, which fills its parent |

**On `<thead><tr>`**

| Class | The CSS it generates | Why this one |
|---|---|---|
| `border-b` | `border-bottom-width: 1px` | Design: `border-bottom:1px solid`. Bottom edge only — `border` would draw all four sides |
| `border-page-line` | `border-color: var(--color-page-line)` | Same token as the card's outer border, so the rule under the headings reads as part of the same system |

**On each of the six `<th>` elements** — all six carry an identical class list

| Class | The CSS it generates | Why this one |
|---|---|---|
| `px-5` | `padding-inline: 20px` | Design: `padding:14px 20px` |
| `py-3.5` | `padding-block: calc(var(--spacing) * 3.5)` = **14px** | `3.5 × 4px`. The header row is deliberately shorter than the body rows will be (`py-4` = 16px), which is what makes it read as a header |
| `text-left` | `text-align: left` | Required, not optional — see call-out 9 |
| `text-[11.5px]` | `font-size: 11.5px` | Design: `font-size:11.5px`. Brackets again: Tailwind's scale has nothing between `text-xs` (12px) and `text-sm` (14px) |
| `font-semibold` | `font-weight: 600` | Design: `font-weight:600` |
| `uppercase` | `text-transform: uppercase` | Design: `text-transform:uppercase`. Crucially **not** typed as capitals in the HTML — see call-out 10 |
| `tracking-wider` | `letter-spacing: var(--tracking-wider)` = **0.05em** | Design: `letter-spacing:.05em`. An exact name match — see call-out 11 |
| `text-page-muted` | `color: var(--color-page-muted)` | Grey, so the headings recede behind the data. Same token as the "6 books" line |

---

### Call-out 7 — Why `overflow-hidden` is load-bearing here

`rounded-xl` rounds the **border**, but a child element's background doesn't know that and will happily paint square corners over the top of it.

```
   ✘ rounded-xl alone                    ✔ rounded-xl + overflow-hidden

   ╭───────────────────────╮             ╭───────────────────────╮
   │███████████████████████│             │███████████████████████│
   │███ child background ██│             │███ child background ██│
   │███████████████████████│             │███████████████████████│
   ╰───────────────────────╯             ╰───────────────────────╯
     ↑ child's square corner                ↑ child clipped to the
       pokes out past the curve               parent's rounded shape
```

You can't see this yet, because the `<tbody>` is empty and nothing inside the card has its own background. **It becomes visible at stop 3**, when the first row gets a hover background — without `overflow-hidden`, hovering the last row would paint two square corners over the card's rounded bottom.

This is the general rule: **`rounded-*` on a parent needs `overflow-hidden` whenever a child paints to the edge.**

### Call-out 8 — `border` sets the width, not the colour

A very common early confusion. In Tailwind these are two independent jobs:

```
      border            →  border-width: 1px      HOW THICK
      border-page-line  →  border-color: …        WHAT COLOUR
      border-b          →  border-bottom-width    WHICH EDGE
```

So the trap is writing the colour alone:

```
   ✘ class="border-page-line"          ✔ class="border border-page-line"
     colour is set, width is 0           1px wide, in your colour
     → nothing appears                   → a hairline appears
```

Why is the width 0 by default? Tailwind's preflight resets `border-width: 0` on every element, precisely so a stray `<table>` or `<input>` never draws a border you didn't ask for. The cost is that you must always opt back in with `border`, `border-b`, `border-2` and so on.

Reading the directional suffixes:

```
      border      all four edges          border-x   left + right
      border-t    top                     border-y   top + bottom
      border-b    bottom          ← used here on the heading row
      border-l    left
      border-r    right
```

### Call-out 9 — Why `<th>` needs `text-left` spelled out

Browsers give `<th>` two built-in styles that `<td>` doesn't get: **bold**, and **centred**. Tailwind's preflight removes the bold but leaves the centring.

```
   without text-left            with text-left
   ┌──────────────────┐         ┌──────────────────┐
   │      NAME        │         │ NAME             │
   │ Klara and the Sun│         │ Klara and the Sun│
   └──────────────────┘         └──────────────────┘
     ✘ heading floats off         ✔ heading sits over
       centre, data sits left       its own column
```

The design explicitly writes `text-align:left` on every `<th>` for exactly this reason. It's one of the few places where you're overriding a browser default rather than adding something new.

### Call-out 10 — `uppercase` instead of typing NAME

The HTML says `Name`. The screen says `NAME`. That gap is deliberate.

```
   ✘ <th>NAME</th>                      ✔ <th class="uppercase">Name</th>

   • screen readers may spell it         • screen readers read "Name"
     out: "N-A-M-E"                      • copying the text gives "Name"
   • copying gives "NAME"                • the capitals are a styling
   • the capitals are baked into           decision you can change in
     your content forever                  one class
```

The principle is **content in the HTML, presentation in the classes.** If a designer later decides the headings shouldn't shout, that's deleting one class — not re-typing six words.

### Call-out 11 — A rare exact match: `tracking-wider`

Every letter-spacing so far has needed brackets. This one didn't, and it's worth seeing why:

```
   DESIGN WANTS      TAILWIND'S NAMED VALUE            RESULT
   -0.01em  (h1)     tracking-tight = -0.025em    ✘  2.5× too tight → brackets
    0.05em  (th)     tracking-wider =  0.05em     ✔  exact → tracking-wider
```

`--tracking-wider: 0.05em` is Tailwind's own default, confirmed in `dist/output.css`. When a design's value happens to land on a named step, **use the name** — it documents intent ("wide tracking") where `tracking-[0.05em]` only documents a number.

Widened letter-spacing on small uppercase text is a standard typographic move, which is *why* both the design and Tailwind arrived at the same value independently. Capitals set at 11.5px run together without it.

---

### Two things deliberately *not* written

| The design says | No class written | Why |
|---|---|---|
| `border-collapse: collapse` on the `<table>` | — | Preflight already sets it (`dist/output.css` line 100). Without collapse, every cell would draw its own separate border and you'd get doubled 2px lines between rows |
| `font-weight` reset on `<th>` | — | Preflight already sets `<th>` to inherit its weight rather than defaulting to bold, so `font-semibold` isn't fighting anything |

Also worth noting what isn't a class at all: `scope="col"` on each `<th>`. That's an HTML attribute, not styling — it tells a screen reader that this heading labels a **column** rather than a row, so when it reads a cell it can announce "Status: Currently Reading". Free, invisible, and impossible to add later with CSS.

---

## Stop 3 — One complete body row

This is the densest stop in the feature. Everything after it is repetition — so if one section is worth reading twice, it's this one.

### What's on screen

```
  ┌───────────────────┬─────────┬──────────┬───────┬────────────────┬───────────┐
  │ Klara and the Sun │ Fiction │ ╭──────╮ │ ★★★★☆ │ Kazuo Ishiguro │ Goodreads │
  │                   │         │ │ Read │ │       │                │           │
  │                   │         │ ╰──────╯ │       │                │           │
  └───────────────────┴─────────┴──────────┴───────┴────────────────┴───────────┘
    text-page-ink       muted     a PILL     4 amber  muted            accent
    font-medium (500)   (13.5px)  span       1 grey   (13.5px)         + underline
                                                                          on hover
```

Three cells hold plain text and one holds a link — but the Status and Score cells each hold a small structure built out of `<span>`s, and those two are where the new ideas live.

### The markup, stripped to its skeleton

```
<tr class="border-b border-page-line hover:bg-page-row-hover">
 ├─ <td> Klara and the Sun          ← plain text, darker + heavier
 ├─ <td> Fiction                    ← plain text, muted
 ├─ <td> └─ <span> Read             ← THE PILL      — a span, not a td style
 ├─ <td> └─ <span> └─ <span>★ × 5   ← THE STARS     — wrapper + 5 children
 ├─ <td> Kazuo Ishiguro             ← plain text, muted (same as Type)
 └─ <td> └─ <a> Goodreads           ← real link
```

The pill and the stars are **spans inside the cell**, never the cell itself. A `<td>` is stretched by its neighbours to fill the row height, so styling the cell directly would produce a full-height coloured block instead of a small pill.

### Every class, one by one

**On `<tr>`**

| Class | The CSS it generates | Why this one |
|---|---|---|
| `border-b` | `border-bottom-width: 1px` | Design: `border-bottom:1px solid`. Every row draws its own bottom rule, which is what separates rows from each other |
| `border-page-line` | `border-color: var(--color-page-line)` | Same hairline token as the card and heading rule |
| `hover:bg-page-row-hover` | `background-color: var(--color-page-row-hover)` on hover | The design's `style-hover` on the row. Needed a new token — see call-out 17 |

**On the Name `<td>`**

| Class | The CSS it generates | Why this one |
|---|---|---|
| `px-5` | `padding-inline: 20px` | Design: `padding:16px 20px` — same horizontal padding as the `<th>` above, so columns line up |
| `py-4` | `padding-block: calc(var(--spacing) * 4)` = **16px** | `4 × 4px`. Taller than the heading row's `py-3.5` (14px) — data gets more room than labels |
| `text-[14.5px]` | `font-size: 14.5px` | Design: `font-size:14.5px`. Brackets: another half-pixel the scale has no name for |
| `font-medium` | `font-weight: 500` | Design: `font-weight:500`. See call-out 16 for why this isn't `font-semibold` |
| `whitespace-nowrap` | `white-space: nowrap` | Design: `white-space:nowrap`. See call-out 15 |
| `text-page-ink` | `color: var(--color-page-ink)` | The darkest text on the page. The title is the row's primary information |

**On the Type and Author `<td>`s** — identical class lists

| Class | The CSS it generates | Why this one |
|---|---|---|
| `px-5` `py-4` | `20px` / `16px` | Same cell padding as every other `<td>` |
| `text-[13.5px]` | `font-size: 13.5px` | Design: `font-size:13.5px`. One pixel smaller than the title — a deliberate hierarchy, not an accident |
| `whitespace-nowrap` | `white-space: nowrap` | Keeps "Kazuo Ishiguro" on one line |
| `text-page-muted` | `color: var(--color-page-muted)` | Grey. Supporting detail, not the headline |

**On the Status, Score and Link `<td>`s**

| Class | The CSS it generates | Why this one |
|---|---|---|
| `px-5` `py-4` | `20px` / `16px` | Padding only. All the styling lives on the `<span>` or `<a>` inside — see the skeleton above |

**On the pill `<span>`**

| Class | The CSS it generates | Why this one |
|---|---|---|
| `inline-block` | `display: inline-block` | Design: `display:inline-block`. Not plain `inline` — see call-out 13 |
| `rounded-full` | `border-radius: calc(infinity * 1px)` | Design: `border-radius:999px`. See call-out 12 |
| `px-3` | `padding-inline: 12px` | Design: `padding:4px 12px` |
| `py-1` | `padding-block: 4px` | `1 × 4px` |
| `text-xs` | `font-size: var(--text-xs)` = **12px** | Design: `font-size:12px`. Exact name match |
| `font-semibold` | `font-weight: 600` | Design: `font-weight:600`. Heavier than the title next to it, because it's small and needs the weight to stay legible |
| `whitespace-nowrap` | `white-space: nowrap` | Stops "Currently Reading" wrapping into two lines inside the pill and bursting its shape |
| `bg-status-read` | `background-color: var(--color-status-read)` | The pale green from feature 0 |
| `text-status-read-ink` | `color: var(--color-status-read-ink)` | The dark green from feature 0. **The pair you predicted in `00-setup.md` — see call-out 12** |

**On the star wrapper `<span>` and the five star `<span>`s**

| Class | The CSS it generates | Why this one |
|---|---|---|
| `inline-flex` | `display: inline-flex` | Design: `display:inline-flex`. Lays the five stars in a row while the wrapper itself stays inline inside the cell |
| `gap-0.5` | `gap: calc(var(--spacing) * 0.5)` = **2px** | Design: `gap:2px`. The scale takes fractions, so even 2px has a name |
| `text-sm` | `font-size: 14px` | Design puts `font-size:14px` on *each* star. Hoisted to the wrapper here — see call-out 14 |
| `text-star-filled` | `color: var(--color-star-filled)` | Amber. On the first four stars |
| `text-star-empty` | `color: var(--color-star-empty)` | Pale grey. On the fifth |

**On the `<a>`**

| Class | The CSS it generates | Why this one |
|---|---|---|
| `text-[13.5px]` | `font-size: 13.5px` | Matches the muted columns |
| `font-medium` | `font-weight: 500` | Design: `font-weight:500`. Slightly heavier than surrounding text, a quiet signal it's clickable |
| `text-accent` | `color: var(--color-accent)` | The near-black shared token — this design doesn't use blue links |
| `hover:underline` | `text-decoration-line: underline` on hover | **Not in the design** — see call-out 18 |
| `focus-visible:outline-2` | `outline-width: 2px` on keyboard focus | Same ring pattern as the Add Book button |
| `focus-visible:outline-offset-2` | `outline-offset: 2px` | Holds the ring clear of the text |
| `focus-visible:outline-accent` | `outline-color: var(--color-accent)` | Same token as the link's own colour |

---

### Call-out 12 — The pill: two tokens, finally spent

`00-setup.md` predicted this exact line of markup. Here it is for real:

```html
<span class="bg-status-read text-status-read-ink">Read</span>
             └──── the pill ────┘ └──── the word ────┘
```

```
   oklch(0.93 0.05 150)   the background — pale
   oklch(0.32 0.09 150)   the text       — dark
              └──┬──┘
             same hue 150. Only the lightness moved.
```

That's the payoff for defining tokens in pairs before any markup existed: writing a readable pill took **zero decisions** here. No contrast checking, no colour picking — just two names that were designed to go together.

**And `rounded-full` has a small surprise in it.** The design says `border-radius:999px`, the ancient trick for "round the ends completely". Tailwind generates something better:

```css
.rounded-full { border-radius: calc(infinity * 1px); }
```

Literal CSS infinity. Both work identically — a radius larger than half the height always produces a semicircle — but `999px` is a guess that happens to be big enough, while `infinity` says what it means. It's why a pill keeps its shape no matter how tall the text inside gets.

```
   ╭──────────╮   the ends are semicircles, always,
   │   Read   │   whatever the font-size or padding
   ╰──────────╯
```

### Call-out 13 — Why `inline-block` and not `inline`

The pill has `padding: 4px 12px`. On a plain `inline` element, **vertical padding renders but doesn't push anything away** — it silently overlaps the lines above and below.

```
   ✘ display: inline                    ✔ display: inline-block

   ─────────────────────                ─────────────────────
   ╭──────────╮ ← padding bleeds        │                   │
   │   Read   │   over the neighbouring │   ╭──────────╮     │
   ╰──────────╯   line, and the row     │   │   Read   │     │
   ─────────────  height ignores it     │   ╰──────────╯     │
                                        │                   │
                                        ─────────────────────
                                          row grows to fit it
```

The rule to remember:

```
   inline         width/height ignored, vertical padding doesn't reserve space
   inline-block   width/height respected, padding reserves space, still sits in a line
   block          same, but forces onto its own line
```

`inline-block` is the one you want for anything pill-, badge- or chip-shaped.

### Call-out 14 — Setting the star size once instead of five times

The design puts `font-size:14px` on **every one of the five stars.** The markup here doesn't:

```html
✘ five copies                      ✔ one copy, inherited
<span class="inline-flex gap-0.5">   <span class="inline-flex gap-0.5 text-sm">
  <span class="text-sm text-…">★      <span class="text-…">★
  <span class="text-sm text-…">★      <span class="text-…">★
  <span class="text-sm text-…">★      <span class="text-…">★
  ...                                 ...
```

`font-size` is an **inherited** property — children take their parent's value unless they override it. So one `text-sm` on the wrapper reaches all five stars. Colour is inherited too, but each star needs its *own* colour (four amber, one grey), so that stays per-star.

```
                text-sm on the wrapper
                         │
        ┌────────┬───────┼───────┬────────┐
        ▼        ▼       ▼       ▼        ▼
      ★ 14px   ★ 14px  ★ 14px  ★ 14px   ★ 14px
      amber    amber   amber   amber    grey     ← colour set individually
```

Worth knowing which properties inherit, because it decides where a class belongs: **`font-size`, `color`, `font-weight`, `letter-spacing` and `line-height` inherit; padding, margin, border, background and display do not.**

### Call-out 15 — What `whitespace-nowrap` is protecting

It's on the Name, Type and Author cells, but **not** on the Status, Score or Link cells. That's deliberate.

```
   ✘ without nowrap, a narrow window     ✔ with nowrap
     wraps a title mid-phrase

   ┌──────────────┐                      ┌────────────────────┐
   │ Klara and    │                      │ Klara and the Sun  │
   │ the Sun      │                      └────────────────────┘
   └──────────────┘                        column widens instead
     row height jumps
```

Tables size their columns to their content, so `nowrap` effectively says *"this column may get wider, but this text will not break."* The cells that don't have it don't need it — the pill has its own `nowrap`, the stars can't wrap, and "Goodreads" is one word.

The cost: on a narrow phone this forces the table wider than the screen. That is a real problem, and it's exactly what feature 3 (responsive) exists to solve.

### Call-out 16 — Three font weights, on purpose

This row uses a third weight, so all three are now on screen at once:

```
   400  normal        Fiction · Kazuo Ishiguro       the muted columns
   500  font-medium   Klara and the Sun · Goodreads  the title and the link
   600  font-semibold NAME TYPE STATUS · Read        headings and the pill
```

The pattern isn't "more important = heavier". It's **smaller text needs more weight to stay readable.** The pill (12px) and column headings (11.5px) are the smallest text on the page and carry the heaviest weight; the 14.5px title only needs 500 to stand out from the 13.5px around it.

### Call-out 17 — The row hover, and `overflow-hidden` finally paying off

One new token was added for this, in `src/input.css`:

```css
--color-page-row-hover: oklch(0.98 0.003 90);
```

Look at where it sits between the two colours already in play:

```
   lighter ◄──────────────────────────────────────────► darker

   1.000        0.985            0.98
   #fff         page-bg          row-hover
   the card     the page         a hovered row
      │            │                 │
      └── the row is barely darker than the card it sits on,
          and still lighter than the page around it
```

That narrow band is the whole design: enough change to confirm "the pointer is on this row", not so much that it looks selected. Feature 2's genuinely-selected row will need to be stronger than this.

**And this is where call-out 7 comes true.** Hover the *last* row in the table, once stop 4 adds more:

```
   ✘ without overflow-hidden          ✔ with overflow-hidden
     on the card                        on the card

   │  hovered row       │             │  hovered row       │
   ╰────────────────────╯             ╰────────────────────╯
   └─ tinted background sticks        └─ tint clipped to the
      out past the rounded               card's curve
      corner as a square block
```

The class was written back at stop 2, before there was anything to clip. That's normal — structural decisions get made before their consequences are visible.

### Call-out 18 — The link's hover is invented too

Same situation as the Add Book button. The design gives the link `color`, `font-weight` and `text-decoration:none`, and **no hover or focus state at all.** Your `coding-standards.md` requires both on anything interactive, so `hover:underline` plus the focus ring is a deliberate addition.

`hover:underline` is the conservative choice: underline-on-hover is the oldest, most universally understood link affordance there is, and it adds no new colour. The alternative — darkening the text — would need a token the design doesn't define.

Note that these links are already underline-free *without* a class, because preflight sets `a { text-decoration: inherit }`, which inherits "none" from the page. So `hover:underline` is turning something *on*, not back on.

---

### Two things deliberately *not* written

| The design says | No class written | Why |
|---|---|---|
| `cursor: pointer` on the `<tr>` | — | In the design, clicking a row opens the drawer. **This project has no JavaScript**, so nothing happens on click. A pointer cursor promising a click that doesn't exist is worse than no cursor change. The hover tint is kept because it's a visual fact of the design; the pointer is dropped because it's a functional lie |
| *(nothing)* — a transition on the row hover | — | The design's row hover has no transition, so the tint switches instantly. `transition-colors` would have been polish beyond the reference. Contrast with the Add Book button, where `transition-opacity` was fair game because the whole hover was already an invention |

---

## Stop 4 — The remaining five rows

**Four new classes. That's the entire stop.** Everything else is stop 3's row pattern, copied five times with different words in it. If stop 3 made sense, there is nothing here you have to work at — which is the point of ordering them this way.

### What's on screen

```
  ┌──────────────────────┬─────────────┬─────────────────────┬───────┐
  │ Klara and the Sun    │ Fiction     │ ╭ Read ╮            │ ★★★★☆ │
  │ Project Hail Mary    │ Sci-Fi      │ ╭ Read ╮            │ ★★★★★ │
  │ Sapiens              │ Non-Fiction │ ╭ Currently Reading╮│ ★★★★☆ │
  │ The Midnight Library │ Fiction     │ ╭ Want to Read ╮    │ ☆☆☆☆☆ │
  │ Educated             │ Memoir      │ ╭ Read ╮            │ ★★★★★ │
  │ Dune                 │ Sci-Fi      │ ╭ Currently Reading╮│ ★★★★☆ │
  └──────────────────────┴─────────────┴─────────────────────┴───────┘
                                          green / orange / blue
```

Six rows, three statuses, and star counts of 0, 4 and 5 — chosen so every branch of the design is on screen at once with nothing left untested.

### Every new class

| Class | The CSS it generates | Why this one |
|---|---|---|
| `bg-status-currently-reading` | `background-color: var(--color-status-currently-reading)` | Pale orange, on Sapiens and Dune |
| `text-status-currently-reading-ink` | `color: var(--color-status-currently-reading-ink)` | Dark orange, the word on top of it |
| `bg-status-want-to-read` | `background-color: var(--color-status-want-to-read)` | Pale blue, on The Midnight Library |
| `text-status-want-to-read-ink` | `color: var(--color-status-want-to-read-ink)` | Dark blue, the word on top of it |

Everything else — `px-5 py-4`, `text-[14.5px]`, `font-medium`, `whitespace-nowrap`, `text-page-ink`, `text-[13.5px]`, `text-page-muted`, `inline-block`, `rounded-full`, `px-3 py-1`, `text-xs`, `font-semibold`, `inline-flex`, `gap-0.5`, `text-sm`, `text-star-filled`, `text-star-empty`, `text-accent`, `hover:underline`, the focus ring, `border-b`, `border-page-line`, `hover:bg-page-row-hover` — is identical to stop 3.

---

### Call-out 19 — All six status tokens, side by side

Feature 0 defined three pairs. All three are now on screen at the same time, and the system is visible in a way it wasn't with one pill:

```
   STATUS               BACKGROUND            INK                  HUE
   ─────────────────────────────────────────────────────────────────────
   Read                 0.93 0.05  150        0.32 0.09 150        150  green
   Currently Reading    0.93 0.07   55        0.40 0.13  55         55  orange
   Want to Read         0.93 0.045 250        0.40 0.10 250        250  blue
                        └────┬────┘           └────┬───┘
                   every background is    every ink is dark
                   lightness 0.93         (0.32–0.40)
```

Read the columns rather than the rows. **The lightness barely moves within a column, and only the hue changes between rows.** That's what makes three differently-coloured pills look like one family instead of three unrelated badges — they're the same design at three angles around the colour wheel.

This is also why `project-overview.md`'s original `#22c55e` / `#f97316` / `#3b82f6` were abandoned in feature 0. Those three hexes are all fully saturated at *different* lightnesses, so they'd have produced three pills of visibly different weight.

### Call-out 20 — The zero-score row, and why `aria-label` does the work

The Midnight Library has a score of 0, so all five stars use `text-star-empty`:

```
   ☆☆☆☆☆     five grey stars
```

To a sighted reader that clearly means "no rating". To a screen reader, the five `<span>`s are meaningless — which is why the wrapper carries the meaning instead:

```html
<span role="img" aria-label="0 out of 5">
  <span aria-hidden="true">★</span>   ← ×5, all hidden from assistive tech
</span>
```

```
   role="img"          "treat this group as a single image"
   aria-label="…"      "…and here is its alt text"
   aria-hidden="true"  "ignore me entirely" — on each star
```

Without this, a screen reader announces "star star star star star" for **every row**, identically, whether the score is 0 or 5. The label is the only thing carrying the actual number. Note it's on the wrapper and the stars are hidden — if both were readable you'd get the label *and* the noise.

This is the same principle as call-out 10's `uppercase`: **the visual form and the meaning are stored separately**, so changing one doesn't silently break the other.

### Call-out 21 — Why six near-identical rows is correct here

Every row repeats about twenty classes. In React you'd write this once and `.map()` over the data. This project deliberately doesn't:

> *"Some duplication across files is expected and fine; the repetition is part of what teaches you the classes."* — `project-overview.md`

There is no partial system, no templating, and no JavaScript to loop with. Typing `px-5 py-4 text-[13.5px] whitespace-nowrap text-page-muted` for the sixth time is the exercise, not a failure of the exercise.

Worth being clear about the trade-off though, because it's real:

```
   ✔ what repetition buys here          ✘ what it would cost in a real app
   ─────────────────────────────        ────────────────────────────────────
   the class list becomes muscle        changing the muted colour means
   memory                               editing 12 separate places
   every row is independently           a typo in row 4 only shows up when
   readable                             someone scrolls to row 4
```

In the working React version of this app, that right-hand column is why the row is a component. Here, the left-hand column is why it isn't.

---

### One thing you might notice at the bottom

Every row carries `border-b`, including the last one — so the final row's bottom border sits directly above the card's own border, and the bottom edge of the card looks very slightly heavier than the top:

```
   │ Dune  │ Sci-Fi │ …  │
   ├────────────────────┤  ← the last row's border-b
   └────────────────────┘  ← the card's own border
```

This is faithful to the design, which also puts `border-bottom` on every row without exempting the last. It's a common thing to "fix" with a `last:border-b-0` variant — but that would be a deviation from the reference, so it stays. Noted here so you know it was seen and decided, not missed.
