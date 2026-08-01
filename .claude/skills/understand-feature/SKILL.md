---
name: understand-feature
description: Writes the plain-language, code-level learning note for a screen-state — one section per component as it is built, then a final polish pass once the feature is merged. For learning purposes, not for modifying code. User-invoked only.
disable-model-invocation: true
---

# Understand Feature

You are producing the code-level explanation of a screen-state, for the user's own learning. The feature spec file path is given as an argument: $ARGUMENTS

This is a read-only, explanatory task. Do not modify any source files. Do not check out or switch branches — the user's current working tree must be left exactly as it is. Everything you write goes into exactly one file: `context/understanding/<same-number-and-name-as-the-spec>.md` (e.g. `context/understanding/01-home-structure-table.md`).

## Two modes

The note is not written in one sitting at the end. It is grown one section at a time, as each component of the screen is built, and finished with a wrapper once the whole feature is merged.

```
   DURING THE BUILD (@context/ai-interaction.md step 4)        AT CLOSE-OUT (step 11)

   component 1 lands ──► append section 1  ─┐
   component 2 lands ──► append section 2   │                 ┌─ opening diagram
   component 3 lands ──► append section 3   ├──► the note ───►├─ (the sections)
   component 4 lands ──► append section 4  ─┘                 └─ closing cheat-sheet

        COMPONENT MODE                                          POLISH MODE
        reads the working tree                                  reads the merged git range
```

Decide which mode you are in from how you were invoked: mid-build after a component, or at feature close-out. If it is genuinely unclear, ask.

## Component mode — append one section

Runs immediately after a component is implemented, before the reviewer is asked to approve it. The work is not committed yet at this point, so **read the working tree, not git history.**

1. **Identify the component** that was just built, and read the markup for it in the file it landed in — plus any `src/input.css` token added for it.

2. **Append a new section** to the note, after the existing sections. Create the file with a title and a one-line orientation sentence if this is the first component. Never rewrite or reorder earlier sections; a later component may make an earlier explanation look incomplete, and that is correct — it was complete at the time.

3. **The section contains, in this order:**
   - A short heading naming the component as the reviewer saw it (e.g. `## Stop 2 — The table frame and its column headers`)
   - An ASCII diagram of the component — its box structure, its element hierarchy, or the thing on screen it produces
   - A table covering **every class in that component**, no exceptions: the class, the CSS it actually generates, and why that one rather than a near neighbour. Routine classes get a row too — `px-8` is not obvious to someone who has never used Tailwind, and skipping it is exactly the gap this mode exists to close
   - Diagram-led call-outs for each concept appearing in the project **for the first time**, one per concept. Check the earlier sections and `context/understanding/00-setup.md` before declaring something new
   - Where a class was deliberately *not* written — because preflight already covers it, or because it would be a no-op — say so and show why, since an absence is invisible in the markup

4. **Verify against the build.** If a class's generated CSS is being described, confirm it by reading `dist/output.css` rather than reciting it from memory. Tailwind's own values are the source of truth, and a wrong pixel value in a teaching note is worse than no note.

## Polish mode — finish the wrapper

Runs at feature close-out, after the merge. The body already exists and **must not be rewritten.** Polish adds only what can't be written until the whole screen is done.

1. **Find the commit range for this feature** using git history, without changing the working tree:
   - `git log --oneline --all --grep="<feature-name-or-keywords>"` to find candidate commits
   - Also `git log --oneline --all --grep="feature/<feature-name>"` in case the merge commit references the branch name
   - Multiple commits means an unsquashed branch — identify the first and last of that range. Exactly one means a squashed merge, and that commit is the full diff
   - If nothing is found automatically, stop and ask the user to confirm the relevant commit hash(es) — do not guess

2. **Read the diff** for that range (`git diff <before>..<after>`, or `git show <commit>`). Do not apply, revert, or check out — only read. Use it to catch anything the per-component sections missed, and to confirm nothing described in the note was later changed.

3. **Cross-reference the spec** at $ARGUMENTS — its Goal, Requirements, and Acceptance Criteria — against what the diff shows, so the note ties "what was asked for" to "what was actually built". Call out any deviation the build made and why.

4. **Add, and only add:**
   - An opening **"the whole feature in one picture"** diagram above the first section, showing how the components fit together
   - A closing **cheat-sheet table** of what this feature makes available to the next one
   - A short closing section on what the next feature starts with
   - If a per-component section is now actively wrong because a later component changed it, correct that one point in place and note what changed — this is the one licence to edit an existing section

## How to write it

Write like a teacher explaining to a student who has never used Tailwind, not like reference documentation. A correct note that reads as a dense wall of prose has failed at its only job. Aim for *fewer ideas, better shown* — it is fine for the file to be physically larger if the extra bytes are diagrams and whitespace rather than sentences.

- **Lead with a picture, not a paragraph.** Open with a diagram of the whole thing — the pipeline, the page layout, the element hierarchy — so the shape is visible before any detail
- **Structure as numbered parts, each answering one question in the student's own words** — "what is `@theme`?", "where did my colours go?", "why two colours per status?" — rather than as topic headings like "Concept 3: token architecture"
- **Draw anything that can be drawn.** Flowcharts for build/scan behaviour, labelled call-outs for unfamiliar syntax, before/after pairs for a decision that went one way instead of another, side-by-side ✔/✘ blocks for a trap that was avoided
- **Use ASCII art inside fenced code blocks, not Mermaid.** VS Code's built-in Markdown preview does not render Mermaid without an extension, so a Mermaid diagram shows up as raw code. ASCII renders everywhere
- **Show the concrete example before the general rule.** Give the actual class string from the markup, then explain what it does
- **Stop and explain syntax a beginner hasn't met**, even when it's incidental to the feature — what `bg-[…]` square brackets mean, why arbitrary values use underscores instead of spaces, how a `md:` prefix reads
- **Close with a cheat-sheet table** of what this feature makes available to the next one
- Follow the Markdown rules in @context/coding-standards.md — in particular, never hard-wrap prose paragraphs

## Notes

- This folder (`context/understanding/`) is a personal learning reference, not part of the always-loaded context in @CLAUDE.md — it will grow large over time and isn't needed for every session.
- The note is committed alongside the component it explains, not held back until after the merge. It is part of the work now, not a postscript to it.
- If the user wants to actually view the code as it existed at that point in history (not just read a diff), suggest `git worktree add ../review-<feature-name> <commit-hash>` instead of checking out in place — this leaves the current working branch untouched. Since these are static HTML files, the worktree copy can be opened directly in a browser with no build step required to view it.
