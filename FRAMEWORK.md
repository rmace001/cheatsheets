# Cheatsheet Framework

Reproducible design system for printable, dark-mode-friendly HTML cheatsheets.

**Goals (in priority order):**
1. **Scannable** — user finds an answer in <5 seconds
2. **Dark-mode friendly** — comfortable on a screen at night
3. **Printer-friendly** — one click to a clean PDF, ideally one Letter page
4. **Self-contained** — single HTML file, no external assets, no JS

---

## 1. Design Tokens

Use these CSS variables verbatim. They map a GitHub-inspired dark palette to a separate light palette used only when printing.

### Dark (screen)
| Token | Value | Use |
|---|---|---|
| `--bg` | `#0d1117` | Page background |
| `--panel` | `#161b22` | Card background |
| `--panel-2` | `#1c232c` | Secondary panel (legend) |
| `--border` | `#30363d` | All borders |
| `--text` | `#e6edf3` | Body text |
| `--muted` | `#8b949e` | Hints, footer, secondary |
| `--accent` | `#7ee787` | Section headers (green) |
| `--accent-2` | `#79c0ff` | Prefix / primary keys (blue) |
| `--accent-3` | `#ffa657` | Shell commands (orange) |
| `--key-bg` | `#21262d` | `kbd` background |
| `--key-border` | `#484f58` | `kbd` border |

### Print (light) — applied inside `@media print`
| Screen role | Print value |
|---|---|
| bg | `#ffffff` |
| text | `#111827` |
| muted | `#6b7280` |
| panel/legend bg | `#f9fafb` / `#f3f4f6` |
| border | `#d1d5db` |
| accent (green) | `#15803d` |
| accent-2 (blue) | `#1d4ed8` |
| accent-3 (orange) | `#9a3412` on `#fff7ed` |

**Rule:** every screen color has a print equivalent. Never leave a `background` or `color` untranslated in print rules — printers default to white-on-white otherwise.

---

## 2. Page Structure

Always in this order:

```
┌─────────────────────────────────────────────┐
│  header   (title + accent-colored topic)    │  ← border-bottom
│                          [meta box ───────] │     (e.g. prefix key)
├─────────────────────────────────────────────┤
│  legend   (color/symbol key — 1 line)       │  ← optional but recommended
├─────────────────────────────────────────────┤
│  grid 3-col                                 │
│   ┌─card──┐ ┌─card──┐ ┌─card──┐             │
│   │ topic │ │ topic │ │ topic │             │
│   ├───────┤ ├───────┤ ├───────┤             │
│   │ rows  │ │ rows  │ │ rows  │             │
│   └───────┘ └───────┘ └───────┘             │
│   ┌─card──┐ ┌─card──┐ ┌─callout─────────┐   │
│   │ topic │ │ topic │ │ recipe / tip    │   │
│   └───────┘ └───────┘ └─────────────────┘   │
├─────────────────────────────────────────────┤
│  footer   (left: context · right: print)    │
└─────────────────────────────────────────────┘
```

**Sizing rules:**
- Outer page: `max-width: 1100px`, padding `28px 32px 40px`
- Grid: `repeat(3, 1fr)` desktop, `repeat(2, 1fr)` at ≤900px, `1fr` at ≤600px
- Gap between cards: `12px`
- Card padding: `12px 14px`

---

## 3. Components

### 3a. Header

```html
<header>
  <div class="title">
    <h1><span class="accent">topic</span> Cheatsheet · Fundamentals</h1>
    <p>One-line description of what this covers.</p>
  </div>
  <div class="prefix">
    <strong>Key concept</strong>
    <!-- e.g. prefix key, base URL, version -->
    <div style="margin-top:4px;">Short note</div>
  </div>
</header>
```

- Title uses `<span class="accent">` to highlight the topic name in green.
- Right-side meta box is **optional** — use it for the single most important context (e.g. tmux prefix, git default branch, vim modes legend).

### 3b. Legend

```html
<div class="legend">
  <span><kbd class="prefix">key</kbd> = meaning</span>
  <span><kbd class="cmd">cmd</kbd> = meaning</span>
  <span><kbd class="then">then</kbd> = sequence</span>
</div>
```

Use when the cheatsheet introduces a notation (color-coded keys, modifier conventions). Skip for purely flat content.

### 3c. Card

```html
<div class="card">
  <h2><span class="num">N</span> Section Title
    <span style="color:var(--muted);font-weight:normal;text-transform:none;font-size:11px;">(optional context)</span>
  </h2>
  <div class="row">
    <div class="desc">Action description<span class="hint">optional secondary line</span></div>
    <div class="keys"><kbd>k</kbd></div>
  </div>
  …
</div>
```

- Numbered badge `<span class="num">` keeps cards visually ordered.
- Subheading hint in muted gray clarifies scope without dominating the header.
- Each row is a 2-col grid: description left, keys right.
- Target **6–8 rows per card** so it fits without scrolling and prints cleanly.
- Target **6 cards + 1 callout** total for a Letter page.

### 3d. Row

Two columns: `1fr auto`, vertically centered, dashed bottom border (last row has none).

`.desc` = the action. Optional `<span class="hint">` adds a second line in muted color for context or warnings.

### 3e. Keys (`kbd` variants)

| Class | Visual | Use for |
|---|---|---|
| `kbd` (default) | gray | Regular keystrokes (`a`, `Enter`, `↑`) |
| `kbd.prefix` | blue border + text | Modifier / prefix keys (`Ctrl`, `Cmd`, tmux prefix) |
| `kbd.cmd` | orange on dark orange | Shell commands or anything you type at a prompt |
| `kbd.then` | borderless gray text | Connectors: "then", "·", "or" |

Always render shortcuts in **chronological order** left → right, with `<kbd class="then">then</kbd>` between steps.

### 3f. Callout

Spans the full grid width by default (`grid-column: 1 / -1`). Use for:
- Quick-start recipe
- "If you only remember 3 things"
- Common pitfalls
- Escape hatch ("how to undo / quit")

Max **one** callout per page so it stays special.

**7-card variant:** when you have 7 content cards, switch the callout to `grid-column: span 2`. It will share the third row with the 7th card instead of forcing a new row with two empty columns. See `git-worktrees.html` for a working example.

### 3g. Footer

```html
<footer>
  <span>Context note about scope/version</span>
  <span>Print / Save as PDF: ⌘P (Mac) · Ctrl+P (Win/Linux)</span>
</footer>
```

Always include the print hint — it's why the page exists.

---

## 4. Print Rules (non-negotiable)

Inside `@media print`:

```css
@page { size: Letter; margin: 0.4in; }
html, body { background: #fff !important; color: #111827 !important; font-size: 10.5px; }
.page { padding: 0; max-width: none; }
.card, .callout { page-break-inside: avoid; }
```

Then override every screen color with its print equivalent (see token table). Use `!important` on print overrides — browsers occasionally cache screen styles for backgrounds.

**Always test:** open print preview before declaring done. If it spills to 2+ pages, reduce font-size to `10px` or trim rows before changing layout.

---

## 5. Content Guidelines

- **Verbs first.** "Detach session" > "Session detachment"
- **One action per row.** Compound actions go in the hint or callout.
- **Keep descriptions ≤ 40 chars** so the row doesn't wrap.
- **Sort each card** by frequency of use (most common at top), not alphabetically.
- **First card = creation / entry.** Last card = exit / escape hatch.
- **Numbered cards** read left-to-right, top-to-bottom — the user can scan in numeric order if lost.

---

## 6. Production Checklist

Before delivering:

- [ ] All shortcuts verified against official docs
- [ ] 6 cards + 1 callout fits the visible grid
- [ ] Print preview renders on **one** Letter page
- [ ] No `background-color` or `color` rule without a print override
- [ ] No external assets (no `<link>`, no `<img>`, no `<script>`)
- [ ] Legend matches every `kbd` variant actually used
- [ ] Footer includes the ⌘P / Ctrl+P hint
- [ ] File saved to `~/Documents/cheatsheets/<topic>.html`

---

## 7. Reference Implementation

See `tmux.html` in this folder. It is the canonical example — copy it as a starting template and replace the content cards.
