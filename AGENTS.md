# AGENTS.md — Cheatsheets Repository

> Single-purpose repo: a collection of printable, dark-mode-friendly HTML cheatsheets the owner uses as fast desk references and PDF exports.

## Owner

- **User:** Rogelio Macedo (`r0m0gg2`)
- **Preferences:** Loves shortcuts, prefers scannable layouts, will likely print or save as PDF.
- **Aesthetic:** GitHub-inspired dark mode on screen, clean black-on-white in print.

## Goals (in priority order)

1. **Scannable** — answer findable in <5 seconds
2. **Dark-mode friendly** — comfortable on a screen at night
3. **Printer-friendly** — one click to a clean PDF, ideally one Letter page
4. **Self-contained** — single HTML file per cheatsheet, no external assets, no build step
5. **Visually consistent** — every cheatsheet uses the shared framework so the set feels coherent

## Repository Layout

```
~/Documents/cheatsheets/
├── AGENTS.md                          ← this file (agent instructions)
├── FRAMEWORK.md                       ← design system (tokens, components, rules)
├── index.html                         ← dark-mode landing page listing every cheatsheet
├── tmux-cheatsheet.html               ← canonical reference implementation
├── git-worktrees-cheatsheet.html
└── <topic>-cheatsheet.html            ← new cheatsheets go here
```

**Naming convention:** `<topic>-cheatsheet.html` — kebab-case, always with the `-cheatsheet.html` suffix.

## Workflow for Adding a New Cheatsheet

1. **Read `FRAMEWORK.md` first.** It defines tokens, components, and print rules. Do not invent new styles.
2. **Copy `tmux-cheatsheet.html`** as a starting template — it's the canonical example.
3. **Replace content cards** (keep the same HTML structure, classes, and `kbd` variants).
4. **Verify against the framework checklist** in `FRAMEWORK.md` §6 before saving.
5. **Register in `index.html`** — add a new `<a class="card">` block with:
   - `href="<topic>-cheatsheet.html"`
   - `data-keywords="…"` (lowercase, space-separated, used for search)
   - icon (single glyph in `.card-icon`, optionally `.blue` or `.orange`)
   - title, one-line subtitle, filename in `.card-meta`
   - color-coded tags (`dark`, `print`, `meta`)
6. **Test print preview** (⌘P) before declaring done. Should fit on one Letter page.

## Design System (summary — see `FRAMEWORK.md` for details)

- **6 cards + 1 callout** is the target layout (or 7 cards with callout spanning 2 cols)
- **3-column grid** desktop, 2-col tablet, 1-col mobile
- **Color-coded `kbd`** to convey meaning at a glance:
  - default gray → regular keys
  - `.prefix` (blue) → modifier / prefix keys
  - `.cmd` (orange) → shell commands
  - `.then` (borderless) → sequence connectors
- **Every screen color must have a print equivalent.** Print styles live inside `@media print` and use `!important` overrides.
- **First card = creation/entry.** Last card = exit/escape hatch. Cards in between sorted by frequency.

## Content Guidelines

- **Verbs first.** "Detach session" > "Session detachment"
- **One action per row.** Compound actions go in the hint or callout.
- **Descriptions ≤ 40 chars** so rows don't wrap.
- **Sort each card by frequency,** not alphabetically.
- **Always include the print hint** in the footer.
- **One callout per page.** Use it for the quick-start recipe or escape hatch.

## Safety / Style Rules

- **No external dependencies.** No CDN links, no fonts, no scripts beyond inline (index.html is the only place inline JS is allowed, and only for filter).
- **No build step.** Hand-authored HTML/CSS only. The owner should be able to open any file directly in a browser.
- **Do not modify `FRAMEWORK.md` casually** — it is the source of truth. Update it only when a new component pattern is introduced and we agree to make it part of the system.
- **Keep `index.html` in sync** every time a new cheatsheet is added.

## Notes on Origin

This repo grew from a single session in the `mmc-agent-workspace` where the owner asked for a tmux cheatsheet. The design that emerged (dark mode + print stylesheet, color-coded `kbd`, numbered cards, callout for the quick-start recipe) was then distilled into `FRAMEWORK.md` so future cheatsheets stay consistent. Treat `tmux-cheatsheet.html` as the canonical reference implementation when in doubt.
