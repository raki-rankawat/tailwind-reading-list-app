# AI Interaction Guidelines

## Communication

- Be concise and direct
- Explain *why* a particular utility or arbitrary value was chosen, not just what was typed — the point of this project is that the human is learning Tailwind, not receiving finished files
- Don't silently fix something the human got visually wrong without saying what was off and why
- Don't add screens, states, or polish not called for in the current feature spec
- Never delete files without clarification

## Workflow

This is the workflow for every feature/screen-state:

1. **Document** - Copy the feature's goal into @context/current-feature.md, status "In Progress"
2. **Branch** - Create a branch for the feature (see Branching below)
3. **Read the design** - Read the relevant part of `Reading List.dc.html` fresh, per @context/project-overview.md — don't work from memory of the old React build
4. **Implement, one component at a time** - Break the screen into its visual components (header, table frame, a single row, the drawer panel, a search card) and build them one at a time, each finished through every layer — structure, spacing, typography, colour, hover/focus — before the next one starts. Pause after each component for the human to look at it in a browser before continuing — this is a learning project, so the human driving the pacing matters more than speed. Slice vertically, never horizontally: a pause covering one small component is a short class list attached to one thing on screen, which is reviewable, whereas a pause covering the whole page in a single layer is a wall of classes however thin that layer is. Before pausing, append that component's section to the screen's note in `context/understanding/` — the explanation belongs at the moment the classes are on screen and fresh, not months later at the end of the feature (see Learning Notes below)
5. **Check in browser** - Open the file directly. There's no build step to fail — "does it look right" is the only test that exists here
6. **Iterate** - Fix anything that doesn't match the design reference
7. **Commit** - Only after the human has looked at it and is happy
8. **Merge** - Merge to main
9. **Delete Branch** - After merge
10. **Polish the note** - The screen's note in `context/understanding/` already exists by now, written component by component during step 4. Finish it rather than write it: add the opening "whole feature in one picture" diagram, the closing cheat-sheet table, and a short section on what the next feature starts with
11. Mark completed in @context/current-feature.md and add to history

Do NOT commit without the human confirming they've actually looked at the result. A file that "should be correct" per the spec but hasn't been eyeballed isn't done — the whole point is building the muscle memory of writing Tailwind and seeing what it produces.

## Branching

One branch per screen-state, named `feature/[screen-name]` (e.g. `feature/home-populated`).

## Commits

- Ask before committing
- Keep commits focused — one screen-state per commit
- Never put "Generated With Claude" or a Co-Authored-By trailer in commit messages

## When Stuck

- If a Tailwind utility doesn't produce what the design reference shows after 2-3 attempts, stop and explain what's not lining up rather than reaching for an arbitrary value or inline style as a quiet workaround
- If the design reference itself is ambiguous for a given screen, say so and ask, rather than guessing

## Code Changes

- Make minimal, targeted changes to the file at hand
- Don't refactor other already-built screens unless asked
- Don't pre-build a state or screen from a later feature "while you're in there"

## Learning Notes

Every screen-state gets an understanding note at `context/understanding/<number>-<name>.md`, and it is never optional — it is written one section at a time, as each component is built, and committed alongside the component it explains.

Each component's section covers **every class in that component** in a table (the class, the CSS it generates, and why that one rather than a near neighbour), followed by diagram-led call-outs for any concept appearing for the first time — e.g. "why `grid-cols-[repeat(auto-fill,minmax(...))]` instead of a fixed `grid-cols-4`", or "why the dark card grid needed its own `@theme` tokens instead of reusing the light-mode ones". Assume the reader has never used Tailwind and stop to explain incidental syntax, not just the feature's headline ideas. Never on app logic, since there isn't any here.

`.claude/skills/understand-feature/SKILL.md` has the full house style — lead with a picture, ASCII art rather than Mermaid, concrete example before general rule.
