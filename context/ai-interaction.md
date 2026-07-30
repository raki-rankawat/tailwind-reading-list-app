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
4. **Implement, one part at a time** - Build the screen's markup and classes incrementally (structure first, then spacing, then color/typography, then responsive/state variants). Pause after each part for the human to look at it in a browser before continuing — this is a learning project, so the human driving the pacing matters more than speed
5. **Check in browser** - Open the file directly. There's no build step to fail — "does it look right" is the only test that exists here
6. **Iterate** - Fix anything that doesn't match the design reference
7. **Commit** - Only after the human has looked at it and is happy
8. **Merge** - Merge to main
9. **Delete Branch** - After merge
10. **Explain** - Ask whether the human wants a short learning note written to `context/understanding/` for this screen (see below) — every time, don't assume
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

Each screen-state's understanding note (if the human asks for one) should focus on the Tailwind concepts it exercised — e.g. "why `grid-cols-[repeat(auto-fill,minmax(...))]` instead of a fixed `grid-cols-4`", or "why the dark card grid needed its own `@theme` tokens instead of reusing the light-mode ones" — not on app logic, since there isn't any here.
