# Cheatsheets

A collection of printable, dark-mode-friendly HTML cheatsheets. Each one is a single self-contained HTML file designed to look great on screen and print cleanly to a one-page PDF.

Open [`index.html`](./index.html) in a browser for the landing page.

## Contents

- [`tmux.html`](./tmux.html) — Sessions, windows, panes, copy mode
- [`git-worktrees.html`](./git-worktrees.html) — Create, list, prune, plus Node/Next.js tips
- [`pr-review.html`](./pr-review.html) — One-page Dijkstra-inspired PR review flow
- [`pr-review-deep-dive.html`](./pr-review-deep-dive.html) — Refinement, invariants, topology, commitments, and review-agent design

## Design

Every cheatsheet uses the shared visual framework documented in [`FRAMEWORK.md`](./FRAMEWORK.md) — same dark palette, same print stylesheet, same component patterns.

## Adding a new cheatsheet

See [`AGENTS.md`](./AGENTS.md) for the workflow. The short version:

1. Copy `tmux.html` as a starting template
2. Replace the content cards
3. Register it in `index.html`
4. Print-preview to verify it fits on one Letter page

## Print to PDF

Open any cheatsheet in a browser and press <kbd>⌘P</kbd> (Mac) or <kbd>Ctrl+P</kbd> (Win/Linux). The print stylesheet swaps to a clean black-on-white layout.
