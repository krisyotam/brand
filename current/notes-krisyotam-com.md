---
name: notes-krisyotam-com-current-design
site: https://notes.krisyotam.com
repo: ~/src/notes.krisyotam.com
status: ground-truth
updated: 2026-08-06
sources:
  - src/app/globals.css
  - src/app/layout.tsx
  - src/lib/theme-context.tsx
  - process.md / README (product intent)
---

# notes.krisyotam.com - current design system

Ground truth for the personal knowledge base. The defining interaction is **Andy Matuschak-style stacked panes**: horizontal scrolling columns that stick and obscure each other as the stack grows. Design decisions optimize for multi-note reading, link previews, backlinks, and bibliography, not for a marketing homepage.

## Product and brand context

notes.krisyotam.com is a networked notebook:

- Notes open as columns in a horizontal stack
- Clicking an internal link pushes another pane rather than replacing the page
- Related notes, outgoing links, backlinks, and BibTeX blocks live at the bottom of a note
- Auth via Clerk for private surfaces; public reading remains calm and clinical

Visual lineage:

- **Evergreen notes** soft clinical greys
- **Andy Matuschak** stacked notes / vertical labels when obstructed
- Shared DNA with krisyotam.com content discipline (justify paragraphs, careful lists) but **system UI type** instead of Noto Serif

Make the surface feel like a tool for thinking: quiet chrome, readable body, obvious active pane, no SaaS dashboard cosplay.

## Priority order

1. Preserve stack geometry (625px panes, sticky overlap, obstructed labels).
2. Preserve note reading measure and type rhythm inside a pane.
3. Preserve link colors (system blue / visited purple) for networked navigation.
4. Keep topbar thin and non-competing with note titles.
5. Prefer CSS in `globals.css` over new component libraries.
6. Do not break keyboard and pointer targets on close / open / search.

## Stack and integration

| Layer | Choice |
|-------|--------|
| Framework | Next.js App Router |
| Styling | Tailwind v4-style `@import "tailwindcss"` + large hand-written CSS in `globals.css` |
| Fonts | Geist Sans + Geist Mono variables from `geist/font`; **runtime body defaults to system UI** |
| Theme | `.dark` class via `ThemeProvider` (`src/lib/theme-context.tsx`) |
| Auth chrome | ClerkProvider wraps the tree |
| Routing | `[[...slug]]` catch-all for note paths |

Body sets `overflow: hidden` so **only the panes region scrolls** (horizontal for stack, vertical inside each column).

## Authorship shell

### Topbar

| Token / class | Value |
|---------------|-------|
| `--topbar-height` | `48px` |
| `.topbar` | flex, `bg-primary`, bottom border, pad `0 1rem`, gap `1rem` |
| `.topbar-name` | 17px / 500, black (light) / white (dark) |
| `.topbar-about` | 15px / 400, `#333` light / `#ccc` dark, hover to full ink |
| `.topbar-btn` | 36×36 hit target, transparent, muted icon, hover tertiary bg |

Topbar actions are icon buttons (search, graph, theme, etc.), not text CTAs.

### Search modal

| Property | Value |
|----------|-------|
| Overlay | `rgba(0,0,0,0.4)`, blur 2px, z-index 1000 |
| Modal width | 560px, max `100vw - 2rem` |
| Radius | 8px |
| Shadow light | `0 16px 48px rgba(0,0,0,0.15)` |
| Shadow dark | `0 16px 48px rgba(0,0,0,0.4)` |
| Input | borderless, bottom hairline, 0.9375rem |
| Result row | title 0.875rem/500 + meta 0.75rem muted |
| Hint kbd | mono 0.625rem, tertiary fill, 2px radius |

### Graph modal

- Overlay 50% black
- Modal `80vw × 80vh`, radius 12px, primary surface
- Graph nodes: muted fill, hover accent, active primary text color
- Labels: sans 10px secondary

## Stacked panes (core layout)

### Scroll container

```css
.panes-scroll-container {
  display: flex;
  flex-grow: 1;
  overflow-x: auto;
  overflow-y: hidden;
  background: var(--bg-canvas);
}
```

Horizontal scrollbar: 8px, square thumb, muted on canvas.

### Column geometry

| Property | Value | Meaning |
|----------|-------|---------|
| Width | **625px** fixed | Canonical note measure |
| Position | `sticky; top: 0` | Stacking trick |
| Background | `--bg-primary` | Paper on canvas |
| Border left | `rgba(0,0,0,0.05)` light / `--border-color` dark | Separation |
| Content pad | **36px 40px** | Internal reading margin |
| Min height | `100vh - topbar` | Full viewport column |

Width transitions use `cubic-bezier(0.19, 1, 0.22, 1)` at 100–200ms.

### Pane states

| State | Visual |
|-------|--------|
| Default | Flat paper, left hairline |
| `.overlay` | Soft shadow `0 0 15px 3px rgba(0,0,0,0.1)` (stronger in dark) |
| `.active` | No extra shadow unless also overlaying |
| `.obstructed` | Content opacity 0; vertical label appears; cursor pointer |

### Obstructed label

| Property | Value |
|----------|-------|
| Label strip width | 40px |
| Writing mode | `vertical-lr` |
| Type | 17px / 500 (600 when active) |
| Letter spacing | 0.03em |
| Line height | 40px (matches strip) |
| Offset | `margin-top: 36px` |

This vertical title is a **signature** of the product. Do not replace with tiny tabs without a deliberate redesign.

### Close control

- 1.5rem hit target, top-right inside pane
- Opacity 0 until pane hover
- Muted → primary on hover

## Color

### Named CSS variables (primary system)

Light ("Evergreen-inspired soft clinical palette"):

| Token | Hex / value | Role |
|-------|-------------|------|
| `--bg-canvas` | `#f0f0f0` | Area behind panes |
| `--bg-primary` | `#ffffff` | Pane / topbar surface |
| `--bg-secondary` | `#fafafc` | Subtle panels, backlinks tray |
| `--bg-tertiary` | `#f0f0f4` | Hover fills, code wash |
| `--text-primary` | `#333333` | Body ink |
| `--text-secondary` | `#666666` | Secondary |
| `--text-muted` | `#888888` | Meta |
| `--border-color` | `#e0e0e0` | Hairlines |
| `--link-color` | `rgba(10, 132, 255, 1)` | Links |
| `--link-visited` | `#af52de` | Visited |
| `--accent` | `#333333` | Active marks |
| `--accent-muted` | `#666666` | Soft accent |

Dark:

| Token | Value |
|-------|-------|
| `--bg-canvas` | `hsl(0 0% 5%)` |
| `--bg-primary` | `hsl(0 0% 9%)` |
| `--bg-secondary` | `hsl(0 0% 12%)` |
| `--bg-tertiary` | `hsl(0 0% 16%)` |
| `--text-primary` | `hsl(0 0% 100%)` |
| `--text-secondary` | `hsl(0 0% 75%)` |
| `--text-muted` | `hsl(0 0% 50%)` |
| `--border-color` | `hsl(0 0% 20%)` |
| `--link-color` | `#79a8ff` |
| `--link-visited` | `#f78fe7` |
| `--accent` | white |
| `--accent-muted` | 75% grey |

### shadcn-compatible HSL bridge

Also defines `--background`, `--foreground`, `--card`, `--primary`, `--muted`, `--border`, `--radius: 0.125rem` (note: **sharper** than main site 0.5rem) for any Tailwind/shadcn pieces.

Mapped into `@theme inline` for Tailwind v4 color tokens.

### Hardcoded ink exceptions (intentional)

Some title/backlink colors are fixed hex rather than variables:

| Element | Light | Dark |
|---------|-------|------|
| `.note-title` | `#333333` | `#cccccc` |
| `.related-title` | `#333333` | (inherits / same block) |
| `.backlink-source` | `#000000` | needs contrast care in dark |
| `.backlinks-title` | `#888888` | |

When theming, audit these hardcodes; do not add more without variables.

## Typography

### Runtime stacks (`:root` + `body`)

```css
--font-body: -apple-system, BlinkMacSystemFont, system-ui, sans-serif;
--font-serif: same as body; /* intentionally not a literary serif */
--font-sans: same as body;
--font-mono: "JetBrains Mono", "Fira Code", Menlo, monospace;
/* body also: var(--font-geist-mono) first for mono */
```

Geist variables are loaded on `<body>` for availability, but **note reading uses system UI**, matching Evergreen/Andy aesthetics more than the main site's Noto Serif.

### Note title

| Property | Value |
|----------|-------|
| Size | **28px** |
| Line height | 34px |
| Weight | 600 |
| Margin below header block | 1.5rem |

### Note body (`.note-content`)

| Property | Value |
|----------|-------|
| Size | **17px** |
| Line height | **24px** |
| Weight | 400 |
| Color | `#333333` (light) |
| Features | kern, liga, calt |
| Paragraph align | justify / last left |
| Paragraph margin | 0 0 12px |

#### Headings (Andy style: normal weight)

| Level | Size |
|-------|------|
| h1 | 1.625rem |
| h2 | 1.5rem |
| h3 | 1.25rem |
| h4 | 1.125rem |
| h5 | 1rem |
| h6 | 0.875rem, secondary color |

All: weight **normal**, tracking -0.02em, margin-top 1.5em (h1 top 0).

#### Inline styles

| Element | Treatment |
|---------|-----------|
| strong | 600 |
| em | italic |
| u | underline, offset 0.15em |
| s/del | strike + muted color |
| code | JetBrains Mono, 0.85em, tertiary bg, 3px radius |
| pre | 13px/17px, secondary bg, 4px radius, pad 4px 8px |
| blockquote | italic, left border 3px, secondary bg, pad 0.5rem 1rem |
| a | link-color, no underline default; visited purple |
| lists | disc/circle alternating; li line-height 24px |
| table | bordered, header secondary bg, even row wash |
| hr | 1px border-color |
| img | centered, max-width 100%, radius 4px |
| kbd | mono, border, light shadow |
| KaTeX display | margin 1rem 0, horizontal scroll if needed |

### Related / links / backlinks / bibliography

| Block | Signature |
|-------|-----------|
| `.related-section` | Top border, title ~25px/400, table 13px with uppercase 11px headers |
| `.links-section` | Chip list: 0.8125rem chips, secondary bg, 4px radius |
| `.backlinks-section` | Padded tray `bg-secondary`, radius **12px**, two-column entries |
| `.bib-section` | Radius **16px**, mono fields 12.5px, citekey emphasis |

Backlink entries:

- ~50% width columns
- Source 12px, context 12px clamped to 3 lines
- Hover wash `rgba(0,0,0,0.05)` / white 0.05 dark

### Link preview popover

| Property | Value |
|----------|-------|
| Scale | 0.8 of a 625px pane |
| Radius | 8px |
| Max height | 400px |
| Shadow | layered drop-shadows (stronger in dark) |
| Title | 28px/600 |
| Body | 17/24 like note content |
| Links inside | non-interactive (`pointer-events: none`) |

Preview is a **scaled miniature note**, not a generic Open Graph card.

## Spacing summary

| Context | Values |
|---------|--------|
| Pane padding | 36×40 |
| Title → body | 1.5rem header margin |
| Paragraphs | 12px |
| Section breaks (related/links) | 2.5rem top + 1.25rem pad-top |
| Backlinks / bib trays | 16px pad |
| Topbar | 48px height, 1rem inline pad |
| Chips gap | 0.5rem |

## Radius summary

| Element | Radius |
|---------|--------|
| shadcn token | 0.125rem |
| Search modal | 8px |
| Preview popover | 8px |
| Graph modal | 12px |
| Backlinks tray | 12px |
| Bib tray | 16px |
| Buttons (topbar) | 4px |
| Inline code | 3px |
| Pre | 4px |
| Link chips | 4px |

Notes is **softer** than main-site content chrome (which likes radius 0 on code).

## Motion

| Interaction | Timing |
|-------------|--------|
| Pane width | 100ms / 200ms, cubic-bezier(0.19, 1, 0.22, 1) |
| Overlay shadow | 100ms linear |
| Obstruct opacity | 75–150ms linear |
| Button/chip hover | 100–150ms |
| html | `scroll-behavior: smooth` (mostly vertical inside panes) |

No decorative page transitions between notes; the stack **is** the transition.

## Components and patterns

### Empty state

Centered muted text, h2 1.25rem/500 secondary.

### Flashcards table

Full-width bordered table, 0.875rem, hover tertiary row.

### Bibliography search

Square inputs (`border-radius: 0`), 13px, focus → muted border + tertiary bg.

### Pagination for sections

11px page numbers, compact prev/next icon buttons.

## Accessibility

- 36px topbar icon targets
- Close buttons appear on hover; ensure focus styles remain available for keyboard users when extending
- Visited link color supports network navigation memory
- Graph and search modals need focus traps when enhanced; do not remove Escape-to-close patterns
- Contrast: primary text `#333` on white is intentional soft ink (not pure black); do not lighten further

## Anti-patterns

Do not:

- Change pane width casually (625px is load-bearing for muscle memory and previews)
- Replace system body with Noto Serif without a product decision (that is main-site literary mode)
- Put a marketing hero above the stack
- Use heavy card grids for notes lists inside panes
- Introduce framer-motion page transitions that fight sticky stacking
- Drop link blue for "elegant" underline-only content links without visited state
- Set `body { overflow: auto }` and break the pane scroll model

## Agent checklist

1. Does the change respect 625px pane width and sticky stacking?
2. Light and dark variables both updated (including link colors)?
3. New UI using clinical greys, not main-site pastels (unless intentionally shared)?
4. Type: system UI for reading, mono only for code/bib?
5. Any hardcoded hex audited for dark mode?
6. Update this doc if stack geometry or tokens change.

## Related

| Path | Why |
|------|-----|
| `src/app/globals.css` | Almost the entire design system |
| `src/app/layout.tsx` | Font variable injection, Clerk, theme |
| `../design.md` | Shared brand kit |
| `krisyotam-com.md` | Sibling literary site (different type model) |
