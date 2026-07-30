# Feature: Home Page — Drawer, Status Control & Delete Popup

## Status

Not Started

## Goal

Add the book detail drawer to `home.html`, shown already open and overlaid on the populated table from feature 1 — including its status control and delete confirmation, built together since they're one continuous piece of UI, not three separate screens.

## Requirements

- Below the populated table from feature 1, add a section showing the **populated table again** (feature 1's markup, copied) with the drawer open over it:
  - A backdrop dimming the table
  - A right-hand panel (~400px on desktop) with: cover image, title, type, author, link, score, and a status control
  - The status control is a real `<select>` styled as a plain full-width control per the design (your old build's first attempt made it a pill and had to correct that once the design reference was actually checked — check it here too rather than assuming)
  - A close `×` control (styled, not wired)
  - One table row visually marked "selected" to sell the idea this row is what's open
- In the same section, show the delete confirmation popup as already open on top of the drawer: small centered card, drawer fields dimmed/blurred behind it, "Cancel" and "Delete Book" buttons using the design's two delete reds

## Acceptance Criteria

- Panel width, content order, and spacing match `Reading List.dc.html`'s drawer section
- The status control matches whatever the design actually shows (confirm plain select vs. pill by reading the file)
- Delete popup renders centered over the drawer panel specifically, not the whole viewport
- Native `<select>` semantics preserved — no custom dropdown markup

## Depends On

- Feature 1 (table to overlay onto)

## Design Reference

`Reading List.dc.html`'s drawer, status control, and delete-confirmation sections. Read each fresh — don't port `BookDrawer.tsx`.

## Notes

If the design deletes on a single click with no confirmation step at all (your old build's history notes the confirm was your own addition, not the design's), that's a deliberate, worth-keeping deviation either way — record which way you went in `context/current-feature.md`.

No mobile version yet — that's feature 3.
