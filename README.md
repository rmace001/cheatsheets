# Cheatsheets

A collection of printable, dark-mode-friendly HTML cheatsheets. Each one is a single self-contained HTML file designed to look great on screen and print cleanly to a one-page PDF.

Open [`index.html`](./index.html) in a browser for the landing page.

## Contents

- [`tmux-cheatsheet.html`](./tmux-cheatsheet.html) — Sessions, windows, panes, copy mode
- [`git-worktrees-cheatsheet.html`](./git-worktrees-cheatsheet.html) — Create, list, prune, plus Node/Next.js tips

## Design

Every cheatsheet uses the shared visual framework documented in [`FRAMEWORK.md`](./FRAMEWORK.md) — same dark palette, same print stylesheet, same component patterns.

## Adding a new cheatsheet

See [`AGENTS.md`](./AGENTS.md) for the workflow. The short version:

1. Copy `tmux-cheatsheet.html` as a starting template
2. Replace the content cards
3. Register it in `index.html`
4. Print-preview to verify it fits on one Letter page

## Print to PDF

Open any cheatsheet in a browser and press <kbd>⌘P</kbd> (Mac) or <kbd>Ctrl+P</kbd> (Win/Linux). The print stylesheet swaps to a clean black-on-white layout.
