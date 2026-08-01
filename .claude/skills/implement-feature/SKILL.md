---
name: implement-feature
description: Implements a single screen-state from a spec file, following the document -> branch -> implement -> check -> commit -> merge -> delete branch workflow defined in @context/ai-interaction.md. User-invoked only.
disable-model-invocation: true
---

# Implement Feature

You are implementing a single screen-state end-to-end, following the standing workflow in @context/ai-interaction.md. The feature spec file path is given as an argument: $ARGUMENTS

## Steps

1. **Document** - Read the feature spec at $ARGUMENTS. Update @context/current-feature.md: copy the feature name, goals, and requirements from the spec into it, and set Status to "In Progress".

2. **Branch** - Create a new branch named `feature/[feature-name]` (derive the name from the spec file, e.g. `01-home-structure-table.md` -> `feature/home-structure-table`).

3. **Read the design fresh** - Read the relevant section of `Reading List.dc.html` per @context/project-overview.md's Design Reference, before writing any markup. Do not work from a previously-built React version of this screen, even if one exists in another project — re-deriving the Tailwind classes from the visual reference is the point of this exercise, and porting old JSX with the logic stripped out defeats it.

4. **Implement (looped, one component at a time)** - Break the spec's requirements from @context/current-feature.md (sourced from $ARGUMENTS) into discrete parts before starting, where one part is one **visual component** — the header, the table frame and its column headers, a single table row, the drawer panel, one search card. Each part is built through all of its layers at once (structure → spacing → typography → colour → hover/focus) so it lands finished and reviewable on its own. Do not slice the other way, one layer at a time across the whole screen: that makes every pause span the entire page and defeats the point of pausing. Where a component repeats (table rows, search cards), build one complete instance as its own part and the remaining copies as the part after it — that repeat part should introduce close to zero new classes, and reads as reinforcement rather than review. Then, for each part in order:

   a. Implement only that part, following @context/coding-standards.md. Do not add anything beyond the spec's requirements — no extra states, no screens from a later feature.
   b. Append this component's section to `context/understanding/<same-number-and-name-as-the-spec>.md`, creating the file if this is the first component. Read `.claude/skills/understand-feature/SKILL.md` and follow its component mode and its house style — do not call the Skill tool for it, `understand-feature` sets `disable-model-invocation: true` and the call is refused no matter who asked. The section covers every class in this component, not just the interesting ones.
   c. Stop. Show a *short* summary in chat — files touched, the new classes one line each, and what to look at in the browser — then point at the note's new section for the full explanation. Do not restate the note's contents in chat; that is why it was written to a file.
   d. Wait for explicit approval (e.g. "resume", "continue", "looks good") before starting the next part. Do not implement the next part automatically, and do not batch multiple parts into one pause.
   e. If the reviewer requests a change, make it, update the note's section to match, show the updated summary, and wait again before moving on.

   Repeat a-e until every part of the spec is implemented and approved. Only then move to Check.

5. **Check in browser** - There is no build step to pass here — Tailwind's CLI either compiles or errors on an obvious config typo. The actual test is opening the file(s) directly in a browser and comparing against the design reference. Do this yourself if you can render/view the file, and otherwise explicitly ask the user to open it and confirm before proceeding. Do not move on until this confirmation happens.

6. **Iterate** - If something doesn't match the spec or the design reference, fix it now, before committing.

7. **Commit** - Ask for permission before committing. Once approved, invoke `/commit-msg` to generate and run the commit — don't hand-write the commit message here.

8. **Merge** - Merge the feature branch to main.

9. **Delete Branch** - Ask before deleting the branch, then delete it once merged.

10. **Update README** - Update the progress table row for this feature to "Completed" and append a one-line build-log entry describing what was built and any notable Tailwind decisions (arbitrary values used, new `@theme` tokens added, deviations from the design reference).

11. **Polish the note** - `context/understanding/<same-number-and-name-as-the-spec>.md` already exists, built up one section per component during step 4 and committed alongside those components. Do not rewrite it. Read `.claude/skills/understand-feature/SKILL.md` and carry out its polish mode directly — again, not via the Skill tool, since `understand-feature` sets `disable-model-invocation: true`. Polish adds only the wrapper: the opening "whole feature in one picture" diagram, the closing cheat-sheet table of what this feature makes available to the next one, and the short "what feature N+1 starts with" section. Ask before committing it.

    This is no longer a yes/no question — the note is written as the screen is built, so there is nothing to opt into. It must still not touch source files, the spec, `context/current-feature.md`, or the checked-out branch.

12. **Close out** - Update @context/current-feature.md: set Status to "Completed", clear the Goals/Notes content, and append a one-line summary of what was built to the History section.

## Rules

- Never skip the confirmation step before committing or deleting a branch.
- Never skip the browser-check confirmation in step 4 — a file that "should be correct" per the spec but hasn't been looked at is not done.
- If a Tailwind utility genuinely won't produce what the design reference shows after 2-3 attempts, stop and explain what's not lining up rather than reaching for an arbitrary value or an inline `style` as a silent workaround.
- Don't refactor already-built screens and don't add states or screens not in the current spec.
