# 00 — Setup: Tailwind CLI + Core Tokens

A walkthrough of what feature 0 built and why. Assumes you've never used Tailwind v4 before.

**Commits:** `bf15f51` (scaffold), `edd16c9` (tokens) · **Spec:** `context/features/00-setup.md`

---

## The whole feature in one picture

```
   YOU WRITE THIS              TAILWIND MAKES THIS           YOU USE IT HERE
   src/input.css               dist/output.css               home.html
  ┌──────────────────┐        ┌─────────────────────┐       ┌─────────────────────┐
  │ @import          │        │ .bg-page-bg {       │       │ <link rel=stylesheet│
  │   "tailwindcss"; │        │   background:       │       │       href=output>  │
  │                  │  npm   │     oklch(.985 …);  │ <link>│                     │
  │ @theme {         │ ─────► │ }                   │ ─────►│ <body               │
  │   --color-       │  build │ .text-page-ink {    │       │   class="bg-page-bg"│
  │     page-bg: …;  │        │   color: …;         │       │ >                   │
  │ }                │        │ }                   │       │                     │
  └──────────────────┘        └─────────────────────┘       └─────────────────────┘
```

Feature 0 built the **left box** and proved the **middle arrow** works. No page exists yet — that's feature 1.

---

## Part 1 — The two commands

```
npm run build     runs once, then stops          ← use before committing
npm run watch     stays running, rebuilds on save ← use while working
```

Both do the same thing: read `src/input.css`, write `dist/output.css`. `watch` just repeats it every time you hit save (about a one-second delay).

> **Gotcha found during this feature:** `watch` quits instantly if it's started by a script instead of by you typing it. It prints its logo and exits like nothing's wrong. Run it in a real terminal and it behaves.

---

## Part 2 — What `@theme` is

Think of `@theme` as **a box of crayons you name yourself.**

Old Tailwind (v3) kept that box in a JavaScript file called `tailwind.config.js`. **Version 4 — what this project uses — got rid of that file.** The box now lives in your CSS. There is no config file in this project, and there won't be.

You name one crayon:

```css
@theme {
  --color-status-read: oklch(0.93 0.05 150);
}
```

…and Tailwind hands you back a whole set of tools that use it:

```
        --color-status-read
                 │
     ┌───────────┼───────────┬──────────────┐
     ▼           ▼           ▼              ▼
bg-status-read  text-…  border-…       fill-…
(background)    (text)  (border)       (svg fill)
```

You never write those rules yourself. **Name a colour, get the utilities free.**

That's why the coding standards can ban raw hex codes in `class` attributes and have it be realistic — every colour in this project has a name you can type.

---

## Part 3 — "Where did 13 of my 18 colours go?"

This one will bite you later, so it's worth understanding now.

After writing all 18 colours and running the build, only **5** showed up in `dist/output.css`. Nothing was misspelled. Here's why:

```
  npm run build
        │
        ▼
  ┌─────────────────────────────────┐
  │ Read every file in the project. │
  │ Collect anything that looks     │
  │ like a class name.              │
  └────────────────┬────────────────┘
                   │
                   ▼
      Is "bg-status-read" in that list?
                   │
        ┌──────────┴──────────┐
       YES                    NO
        │                      │
        ▼                      ▼
  Write the colour       Skip it. The colour
  into output.css        exists in input.css
                         but NOT in the browser.
```

At that moment the project had **no HTML at all**, so nothing was using any colour. Strictly, zero should have survived. The five that did were an accident: their names appear as *plain text* inside the Markdown files (`coding-standards.md` uses `--color-search-bg` as a naming example). Tailwind's scanner can't tell documentation from markup — it just looks for words shaped like class names.

The moment `scratch.html` used all 18 in real `class` attributes, all 18 appeared.

> **Remember this:** a colour existing in `input.css` does **not** mean it works in the browser. It only appears once some class uses it. If a colour looks wrong in feature 1, check this before you suspect the value.

---

## Part 4 — Why each status has *two* colours, not one

`project-overview.md` says the status colours are `#22c55e` (green), `#f97316` (orange), `#3b82f6` (blue). The design file uses **none of them**. It gives each status a *pair*:

```js
'Read': { bg: 'oklch(0.93 0.05 150)', text: 'oklch(0.32 0.09 150)' }
             └──── the pill ────┘         └──── the word ────┘
```

Same green. Wildly different brightness. Put them on a scale:

```
  darker ◄──────────────────────────────────────────────────► lighter
  0.0                                                          1.0

        0.32                                         0.93
         ●                                            ●
    the word "Read"                            the pill behind it
         └───────────── big gap = you can read it ────────────┘
```

Now picture using **one** colour for both:

```
   TWO colours (what the design does)      ONE colour (the trap)
   ┌────────────────┐                      ┌────────────────┐
   │ ░░░░ Read ░░░░ │  pale pill,          │ ████████████   │  green on green —
   │                │  dark green word     │                │  the word vanishes
   └────────────────┘  ✔ readable          └────────────────┘  ✘ unreadable
```

So each status needs a background **and** an ink:

```css
--color-status-read:     oklch(0.93 0.05 150);   /* the pill  */
--color-status-read-ink: oklch(0.32 0.09 150);   /* the word  */
```

3 statuses × 2 roles = **6 tokens**. In feature 1 a pill will be written:

```html
<span class="bg-status-read text-status-read-ink">Read</span>
```

---

## Part 5 — Reading an `oklch()` colour

You've probably only used hex (`#22c55e`). This project uses `oklch` because the design file does. It's easier than it looks:

```
   oklch( 0.93    0.05    150 )
            │       │       │
            │       │       └─── HUE — which colour (0=pink, 150=green, 250=blue)
            │       └─────────── CHROMA — how vivid (0 = grey, higher = punchier)
            └─────────────────── LIGHTNESS — how bright (0 = black, 1 = white)
```

Why this matters here: **"same colour but darker" is a change to one number.** Compare:

```
  oklch:   0.93 0.05 150   →   0.32 0.09 150     one number moves. Obvious.
  hex:     #d1f0dc         →   #17492f           three values change. Meaningless.
```

Every colour pair in this project is that same "one number moves" relationship. Hex would hide it.

The same logic shapes the two palettes — **only lightness moves inside each page:**

```
  home.html   — everything is hue 90  (warm grey)
  search.html — everything is hue 260 (cool blue-grey)
```

---

## Part 6 — Why the stripe needed two tokens

The card cover placeholder is a diagonal striped pattern:

```
   ╱╱╱╱╱╱╱╱╱╱╱   two greys alternating every 8px
   ╱╱╱╱╱╱╱╱╱╱╱
```

Here's the catch. In CSS, that pattern is a **background-image**, not a colour:

```
  a colour          →  background-color: …   →  fits in --color-*  ✔
  a gradient/stripe →  background-image:  …   →  does NOT fit      ✘
```

If you forced it into `--color-*`, Tailwind would cheerfully generate nonsense like `text-search-stripe` — text painted with a striped image.

So we stored **the two greys** instead, and let feature 4 build the pattern from them:

```css
--color-search-stripe-a: oklch(0.27 0.014 260);
--color-search-stripe-b: oklch(0.3 0.012 260);
```

```html
class="bg-[repeating-linear-gradient(135deg,var(--color-search-stripe-a)_0_8px,var(--color-search-stripe-b)_8px_16px)]"
                                                                          ↑↑
                                              underscores, not spaces ────┘
```

Two things happening there:

1. **Square brackets** `bg-[…]` mean "I need something Tailwind doesn't have a name for." Use sparingly — the standards say prefer `p-4` over `p-[17px]`.
2. **Underscores replace spaces.** Tailwind splits classes on spaces, so a real space would break the class in two. Tailwind swaps `_` back to a space when building.

This was tested during feature 0 so feature 4 doesn't discover it's impossible.

---

## Part 7 — Why the names look slightly odd

The design file calls it `border`. We called it `--color-page-line`. Why?

Because **Tailwind adds the property name in front**, so a token called `border` stutters:

```
  --color-page-border   →   border-page-border    ✘ says "border" twice
  --color-page-line     →   border-page-line      ✔ reads once
```

The naming pattern is *what it's for*, then *what it is*:

```
  --color- page   - bg
           └─┬─┘   └┬┘
             │      └── the role
             └───────── which page (page = home/light, search = dark)
```

And when a colour is used on **both** pages, it gets no page name at all — `--color-accent`, `--color-star-filled`.

One exception worth noticing: the *empty* star exists twice, as `--color-star-empty` and `--color-search-star-empty`. A faint grey star that reads correctly on a white page disappears on a dark one, so it needs a version per background.

---

## The 18 tokens, at a glance

| Group | Tokens | Used by |
|---|---|---|
| Status pills | `status-read`, `status-currently-reading`, `status-want-to-read` — each with a matching `-ink` | feature 1 (table), feature 2 (drawer) |
| Light page | `page-bg`, `page-ink`, `page-muted`, `page-line` | `home.html` |
| Dark page | `search-bg`, `search-card`, `search-line`, `search-ink`, `search-muted` | `search.html` |
| Stripe bands | `search-stripe-a`, `search-stripe-b` | feature 4 cover placeholder |
| Shared | `accent`, `star-filled`, `star-empty`, `search-star-empty` | both pages |

All are written `bg-…`, `text-…`, or `border-…` in markup — e.g. `bg-page-bg`, `text-status-read-ink`, `border-search-line`.

**Two pairs were deliberately left out**, because they belong to feature 2's drawer: the Delete button's red pair (`oklch(0.75 0.14 25)` / `oklch(0.5 0.16 25)`) and the light stripe pair. Notice the Delete pair is the same "pale background, dark ink" idea as the status pills — just at hue 25 (red).

---

## What feature 1 starts with

A working build, and every colour it needs already named. It can write `bg-page-bg`, `text-page-ink`, `text-page-muted`, `border-page-line`, `bg-status-read` + `text-status-read-ink`, `bg-accent`, `text-star-filled`, `text-star-empty` — without stopping once to invent a colour. That was the entire point of doing feature 0 first.
