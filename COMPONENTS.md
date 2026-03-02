# SplitDecision — Component Reference

This file documents every modular component in the post page layout.
Its purpose is to serve as a reference for future sessions: if asked
"which parts are modular?", consult this file first.

---

## How to read this file

Each component entry has:
- **Selector** — the CSS class(es) that define it
- **HTML tag it lives on** — what element carries the class
- **What it does** — plain description
- **Modular?** — whether it can be reused, moved, or swapped independently
- **Dependencies** — other components it relies on (if any)
- **Variants / modifiers** — BEM-style modifier classes that change its appearance

---

## Components

### 1. `post-body`
- **Tag:** `<article>`
- **What it does:** Centered, max-width reading column for the main post content. Controls horizontal padding and max-width (720px).
- **Modular?** Yes — fully self-contained. Can be dropped on any page with the CSS loaded.
- **Dependencies:** None.
- **Variants:** None currently. Max-width is easily changed via the rule in CSS.

---

### 2. `post-header`
- **Tag:** `<header>` inside `.post-body`
- **What it does:** Groups the tag chip, title, and meta line at the top of the post.
- **Modular?** Yes — can be extracted and reused as a standalone post header block (e.g. on a post listing card).
- **Dependencies:** Should sit inside `.post-body` for spacing, but not required.
- **Variants:** None.

---

### 3. `post-tag`
- **Tag:** `<span>` inside `.post-header`
- **What it does:** Small uppercased category label with a colored border.
- **Modular?** Yes — fully standalone. Drop on any element, anywhere.
- **Dependencies:** None.
- **Variants:** Color is set via `--color-accent-a`. To add category-specific colors, add modifier classes (e.g. `.post-tag--film`, `.post-tag--music`).

---

### 4. `post-title`
- **Tag:** `<h1>` inside `.post-header`
- **What it does:** Large responsive headline using `clamp()` for fluid font sizing.
- **Modular?** Yes — the font-size rule is self-contained.
- **Dependencies:** None.
- **Variants:** None.

---

### 5. `post-meta`
- **Tag:** `<p>` inside `.post-header`
- **What it does:** Small muted line for date, reading time, author, etc.
- **Modular?** Yes — fully standalone.
- **Dependencies:** None.
- **Variants:** None. Add more `·`-separated fields inline as needed.

---

### 6. `post-figure`
- **Tag:** `<figure>`
- **What it does:** Full-width image block with an optional caption. Adds margin above and below, rounds image corners.
- **Modular?** Yes — can be placed anywhere inside `.post-body` or adapted for other contexts.
- **Dependencies:** None.
- **Variants:**
  - **`.post-figure--grid`** — switches the figure to a 2-column CSS grid, turning multiple `<img>` children into a side-by-side pair. Add more columns by changing `grid-template-columns` in CSS.

---

### 7. `takes-section`
- **Tag:** `<section>`
- **What it does:** The outer shell of the split-takes layout. Sets height (`--takes-height: 80vh`), stacks the header bar on top of the columns, and establishes the flex column.
- **Modular?** Partially — it is self-contained as a block but is designed to follow `.post-body` on the page. Height is controlled by the `--takes-height` CSS custom property at `:root`, making it easy to adjust without touching the component itself.
- **Dependencies:** None structurally, but visually intended to follow a `.post-body` section.
- **Variants:** Adjust `--takes-height` in `:root` to change how tall the split section is.

---

### 8. `takes-header`
- **Tag:** `<div>` inside `.takes-section`
- **What it does:** Fixed bar across the top of the split section showing both authors' avatars, names, and verdicts.
- **Modular?** Yes — the header can be restyled or replaced independently without touching the scrollable columns below.
- **Dependencies:** Expects `.takes-header__half`, `.avatar`, `.takes-header__name`, `.takes-header__verdict` children (see below).
- **Variants:** None at the component level.

---

### 9. `takes-header__half`
- **Tag:** `<div>` inside `.takes-header`
- **What it does:** One side of the header bar (left author or right author). Flexbox row containing avatar + name/verdict stack.
- **Modular?** Yes — add or remove author slots by adding more of these.
- **Dependencies:** Lives inside `.takes-header`.
- **Variants:**
  - **`.takes-header__half--right`** — mirrors alignment to `flex-end` for the right-hand author.

---

### 10. `avatar`
- **Tag:** `<div>` inside `.takes-header__half`
- **What it does:** Circular initials badge. Currently shows a single letter.
- **Modular?** Yes — fully standalone. Replace the letter with an `<img>` to use a photo instead (set `border-radius: 50%` on the img, remove the flex centering).
- **Dependencies:** None.
- **Variants:** Border color is set inline by the parent's context (`.takes-header__half:first-child .avatar` vs `.takes-header__half--right .avatar`). To make avatars fully independent, promote the border color to a modifier class.

---

### 11. `takes-header__verdict`
- **Tag:** `<span>` inside `.takes-header__half`
- **What it does:** Small label showing the author's overall verdict (e.g. "Loved it", "Mixed feelings").
- **Modular?** Yes — standalone span.
- **Dependencies:** None.
- **Variants:**
  - **`.verdict--hot`** — indigo color (positive).
  - **`.verdict--cold`** — amber color (negative/mixed).
  - Add more modifier classes for additional sentiment states.

---

### 12. `takes-columns`
- **Tag:** `<div>` inside `.takes-section`
- **What it does:** The flex container that holds the two scrollable panes side by side. Sets `overflow: hidden` to clip both children to the container height, which is what enables independent scrolling.
- **Modular?** Yes — the scroll mechanism is entirely in this container + the `.take` children. To add a third column, add another `.take` and `.takes-rule` pair.
- **Dependencies:** Must be inside a height-constrained parent (`.takes-section` provides this).
- **Variants:** None.

---

### 13. `take`
- **Tag:** `<div>` inside `.takes-columns`
- **What it does:** A single independently scrollable column. `overflow-y: auto` does the work; `flex: 1` makes both columns equal width.
- **Modular?** Yes — the scroll behavior is self-contained. Width is controlled by `flex: 1`; override with a fixed width or different flex value to make columns unequal.
- **Dependencies:** Must be inside `.takes-columns` (or any `overflow: hidden` flex/grid container with a defined height).
- **Variants:**
  - **`.take--left`** — left column; accent color is indigo (`--color-accent-a`).
  - **`.take--right`** — right column; accent color is amber (`--color-accent-j`).
  - To add a third author, add `.take--center` (or a new modifier) with its own accent color.

---

### 14. `takes-rule`
- **Tag:** `<div>` between `.take` elements
- **What it does:** 1px vertical divider between columns.
- **Modular?** Yes — add one between any two `.take` columns. Remove entirely for a gutter-free layout.
- **Dependencies:** None.
- **Variants:** None. Change `background` to change divider color.

---

### 15. `take__heading`
- **Tag:** `<h2>` inside `.take`
- **What it does:** Author section title with a colored bottom border that matches the column's accent color.
- **Modular?** Yes — inherits accent color from the parent `.take--left` / `.take--right` context.
- **Dependencies:** Relies on parent `.take--left` or `.take--right` for border color.
- **Variants:** None.

---

### 16. `take__quote`
- **Tag:** `<blockquote>` inside `.take`
- **What it does:** Pull-quote with a left accent border. Border color matches the column's accent.
- **Modular?** Yes — inherits color from parent `.take` variant.
- **Dependencies:** Relies on parent `.take--left` or `.take--right` for border color.
- **Variants:** None.

---

### 17. `take__subheading`
- **Tag:** `<h3>` inside `.take`
- **What it does:** Small all-caps label for sub-sections within a take (e.g. "Highlights", "Concerns").
- **Modular?** Yes — standalone.
- **Dependencies:** None.
- **Variants:** None.

---

### 18. `take__list`
- **Tag:** `<ul>` inside `.take`
- **What it does:** Unstyled list with a custom `–` dash pseudo-element replacing the default bullet.
- **Modular?** Yes — standalone. Can be used inside or outside a `.take`.
- **Dependencies:** None.
- **Variants:** None.

---

## CSS Custom Properties (global knobs)

These are defined on `:root` and act as the primary configuration layer.
Change them to retheme or resize without touching individual component rules.

| Property             | Default      | Controls                                      |
|----------------------|--------------|-----------------------------------------------|
| `--color-bg`         | `#0f0f13`    | Page background                               |
| `--color-surface`    | `#1a1a24`    | Header bar / elevated surfaces                |
| `--color-border`     | `#2e2e3e`    | Dividers, scrollbar, list borders             |
| `--color-text`       | `#d4d4d8`    | Body copy                                     |
| `--color-muted`      | `#71717a`    | Captions, meta, subheadings, muted labels     |
| `--color-accent-a`   | `#6366f1`    | Left-column (Alex) accent — indigo            |
| `--color-accent-j`   | `#f59e0b`    | Right-column (Jordan) accent — amber          |
| `--color-heading`    | `#f4f4f5`    | Titles and strong text                        |
| `--takes-height`     | `80vh`       | Height of the entire split-takes section      |
| `--font-sans`        | Georgia      | Body / prose font                             |
| `--font-ui`          | system-ui    | UI labels, meta, tags                         |

---

## Instructions for future sessions

When asked "which parts are modular?" or "can I reuse X?":

1. Open this file and find the component by name or selector.
2. Check the **Modular?** field — if Yes, the component can be lifted and reused with just its CSS class.
3. Check **Dependencies** — if a component depends on a parent class for styling (e.g. accent colors), note that the parent class or its CSS variables must travel with it.
4. Check **Variants** — modifier classes expand what a component can do without changing its base rules.
5. The **CSS Custom Properties** table is the fastest way to retheme or resize the whole page without touching individual components.
