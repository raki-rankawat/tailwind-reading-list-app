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

4. **Implement (looped, one part at a time)** - Break the spec's requirements from @context/current-feature.md (sourced from $ARGUMENTS) into discrete parts before starting — typically: structure/semantics first, then spacing/layout, then color/typography, then any state variants (hover/focus). Then, for each part in order:

   a. Implement only that part, following @context/coding-standards.md. Do not add anything beyond the spec's requirements — no extra states, no screens from a later feature.
   b. Stop. Show a short summary of what changed (files touched, notable class choices and why) for this part only.
   c. Wait for explicit approval (e.g. "resume", "continue", "looks good") before starting the next part. Do not implement the next part automatically, and do not batch multiple parts into one pause.
   d. If the reviewer requests a change, make it, show the updated summary, and wait again before moving on.

   Repeat a-d until every part of the spec is implemented and approved. Only then move to Check.

5. **Check in browser** - There is no build step to pass here — Tailwind's CLI either compiles or errors on an obvious config typo. The actual test is opening the file(s) directly in a browser and comparing against the design reference. Do this yourself if you can render/view the file, and otherwise explicitly ask the user to open it and confirm before proceeding. Do not move on until this confirmation happens.

6. **Iterate** - If something doesn't match the spec or the design reference, fix it now, before committing.

7. **Commit** - Ask for permission before committing. Once approved, invoke `/commit-msg` to generate and run the commit — don't hand-write the commit message here.

8. **Merge** - Merge the feature branch to main.

9. **Delete Branch** - Ask before deleting the branch, then delete it once merged.

10. **Update README** - Update the progress table row for this feature to "Completed" and append a one-line build-log entry describing what was built and any notable Tailwind decisions (arbitrary values used, new `@theme` tokens added, deviations from the design reference).

11. **Explain** - Ask the user, as a plain yes/no question, whether they want the code-level explanation of what shipped written to `context/understanding/`. Writing a learning note is not part of building the screen — but ask every time, so the step is never silently skipped. Then:

    - **No** — go straight to close out. Don't press.
    - **Yes** — do not call the Skill tool for it. `understand-feature` sets `disable-model-invocation: true`, so that call is refused no matter who asked. Read `.claude/skills/understand-feature/SKILL.md` and carry out its steps directly instead, honouring its rules: it produces exactly one new file, `context/understanding/<same-number-and-name-as-the-spec>.md`, and must not touch source files, the spec, `context/current-feature.md`, or the checked-out branch. Ask before committing it.

    Placement matters: it reads the merged git history, so it cannot run before the merge, and belongs after the README update for the same reason as the original project — a doc-only note landing inside the commit range the next feature's diff would scan is noise worth avoiding.

12. **Close out** - Update @context/current-feature.md: set Status to "Completed", clear the Goals/Notes content, and append a one-line summary of what was built to the History section.

## Rules

- Never skip the confirmation step before committing or deleting a branch.
- Never skip the browser-check confirmation in step 4 — a file that "should be correct" per the spec but hasn't been looked at is not done.
- If a Tailwind utility genuinely won't produce what the design reference shows after 2-3 attempts, stop and explain what's not lining up rather than reaching for an arbitrary value or an inline `style` as a silent workaround.
- Don't refactor already-built screens and don't add states or screens not in the current spec.
