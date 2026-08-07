---
name: krisyotam-com-current-design
site: https://krisyotam.com
repo: ~/src/krisyotam.com
status: ground-truth
updated: 2026-08-06
sources:
  - src/app/styles/themes/default.css
  - src/app/styles/themes/solarized.css
  - src/app/styles/themes/plan9.css
  - src/app/styles/core/content.css
  - src/app/globals.css
  - src/app/layout.tsx
  - tailwind.config.js
  - .doc/audit/design-audit.md
  - .agents/AGENTS.md
---

# krisyotam.com — current design system

This document describes **how the live main site actually works**, not a wishlist. Agents building or restyling UI for krisyotam.com must treat this as authority over generic model defaults. When this conflicts with root `design.md`, prefer this file for site-local work and propose kit changes separately.

## Product and brand context

krisyotam.com is Kris Yotam's primary public archive: essays, papers, notes, fiction, reviews, tracking (film, music, books, anime, games, sports, globe), law/docs, and misc tools. It is a **long-form content machine** with serious generalist readers, not a SaaS marketing site.

Philosophy (from project AGENTS.md): FSF, Unix Way, Suckless, Plan 9. SQLite + MDX. Static by default. JS bundle only shrinks. Components do one thing.

Make the surface:

- Precise, matte, paper-like
- Neutral (pure greys), not tinted
- Dense when content earns density, quiet when it does not
- Editorial for prose, tool-like for tracking tables

Never manufacture confidence with hype, decoration, neon AI aesthetics, or generic SaaS card grids.

## Priority order

1. Preserve content fidelity (MDX, citations, metadata, dates, epistemic fields).
2. Preserve reading comfort in `.content` / `.essays-content`.
3. Preserve semantic tokens (`background`, `foreground`, pastels) and theme classes.
4. Prefer spacing and type hierarchy over new surfaces.
5. Prefer existing UI primitives under `src/components/ui` over new dependencies.
6. Keep the JS bundle equal or smaller.

## Stack and integration

| Layer | Choice |
|-------|--------|
| Framework | Next.js App Router |
| Styling | Tailwind + large CSS layers in `globals.css` and theme files |
| Components | shadcn-style primitives (`components.json`), local `src/components/ui` |
| Fonts | `next/font/local` in `src/app/layout.tsx` |
| Theme | `class` dark mode via `ThemeProvider`, `DarkModeScript` before paint |
| Data | SQLite caches; MDX sources |
| Images | CDN on STARGATE, not repo `public/images` for content photography |

Do not introduce a parallel token system. Extend CSS variables in theme files and map them through `tailwind.config.js` when needed.

## Authorship shell

### Document chrome

- Root layout: `min-h-screen flex flex-col`, grow main, `GlobalFooter`.
- Theme provider: `attribute="class"`, `defaultTheme="system"`, `storageKey="theme"`.
- Favicon / apple touch / webmanifest / RSS alternate in `<head>`.
- Theme color meta: light `#ffffff`, dark `#0c0c0c`.

### Navigation and search

Multiple search surfaces exist (documented in design audit):

| Variant | Location | Character |
|---------|----------|-----------|
| Global overlay | Cmd/Ctrl+K, `search-overlay.tsx` | Modal ~600px, filter tabs (all/writing/reference/poetry), 300ms debounce |
| Search page | `/search` | Full-page, same filters |
| Inline list search | content listing nav | `h-9`, left Search icon, instant parent-driven filter |

When adding search UI, match existing filter tabs and debounce behavior rather than inventing a third pattern without consolidating.

### Footer

Global footer via `FooterProvider` + `GlobalFooter`. Site-local pages may still show content metadata footers; prefer one quiet ownership line over repeated document-state banners.

## Grid, shells, and alignment

### Responsive layout tokens (`globals.css`)

```css
:root {
  --site-gutter: 1rem;
  --site-block-start: 2rem;
  --site-block-end: 0;
  --content-measure: 42rem;
  --home-measure: 48rem;
  --wide-measure: 75rem;
}
```

Breakpoints adjust gutters and block start:

| Breakpoint | Gutter | Block start | Notes |
|------------|--------|-------------|-------|
| default | 1rem | 2rem | |
| ≥768px | 2rem | 3rem | |
| ≥1200px | 3rem | 4rem | |
| ≥1800px | 4rem | 5rem | content 44rem, home 52rem |

### Shell classes

| Class | Measure | Use |
|-------|---------|-----|
| `.content-shell` | `--content-measure` | Reading pages |
| `.home-shell` | `--home-measure`, tighter top block (1.9rem) | Home |
| `.wide-shell` | `--wide-measure` | Tracking, tables, wide tools |
| `.site-shell` | generic shell measure | Shared |

Shells are `width: min(100%, var(--shell-measure))`, centered, with inline gutter padding.

### Alignment rules

- Reading prose stays in the content measure.
- Tables and tracking UIs may use wide shell full width.
- Equivalent list rows share type roles, meta positions, and hover treatment.
- Do not strand a wide table beside a narrow prose rail; put intro above, table full width.

## Color system

### Philosophy

From `default.css` header:

> "100 Whites · 100 Blacks" — pure neutral (0° 0%), matte, zen.  
> 12 pastel hues × 7 steps. Paper-like surfaces, suckless minimalism, Japanese tonal naming. All HSL. Zero hue. Zero saturation. Just lightness.

### 100 Whites (light surface ladder, selected)

Named Japanese whites (HSL lightness only):

| Token | Lightness | Name sense |
|-------|-----------|------------|
| `--white-shironeri` | 99.5% | pure white silk |
| `--white-gofun` | 99% | oyster shell |
| `--white-washi` | 97.5% | rice paper |
| `--white-hakushi` | 97% | blank paper |
| `--white-shiro` | 96% | white |
| `--white-keshizumi` | 92% | cold ash |
| `--white-ginnezumi` | 90% | silver mouse |
| `--white-sunago` | 88% | sand |
| `--white-kurohai` | 80% | dark ash |

Full ladder: `src/app/styles/themes/default.css`.

### 100 Blacks (dark surface ladder, selected)

| Token | Lightness | Name sense |
|-------|-----------|------------|
| `--black-sumi` | 4% | ink |
| `--black-shikkoku` | 7% | lacquer black |
| `--black-tetsu` | 9% | iron |
| `--black-kenpo` | 12% | sword-law black |
| `--black-yukoku` | 14% | deep dark |
| `--black-kurogane` | 17% | black iron |
| `--black-oboro` | 44% | hazy moon |

### Semantic tokens — light

| Token | Value | Role |
|-------|-------|------|
| `--background` | `0 0% 100%` | pure white canvas |
| `--card` | `var(--white-gofun)` | 99% |
| `--popover` | `var(--white-hakushi)` | 97% |
| `--secondary` | `var(--white-ginnezumi)` | 90% |
| `--muted` | `var(--white-keshizumi)` | 92% |
| `--accent` | `var(--white-shiro)` | 96% |
| `--foreground` | `0 0% 20%` | matte charcoal |
| `--muted-foreground` | `0 0% 50%` | secondary text |
| `--primary` | `0 0% 10%` | primary action |
| `--primary-foreground` | `0 0% 95%` | on primary |
| `--destructive` | `0 70% 55%` | errors |
| `--border` | `var(--white-sunago)` | 88% |
| `--input` | `var(--white-ginnezumi)` | 90% |
| `--ring` | `0 0% 40%` | focus |
| `--radius` | `0.5rem` | default radius |
| `--radius-square` | `0` | square chrome |
| `--link` | `rgba(10, 132, 255, 1)` | iOS systemBlue |
| `--link-visited` | `#af52de` | iOS systemPurple |

### Semantic tokens — dark (`.dark`, `[data-theme="dark"]`)

| Token | Value |
|-------|-------|
| `--background` | `var(--black-shikkoku)` (7%) |
| `--card` / `--popover` | `var(--black-tetsu)` (9%) |
| `--secondary` | `var(--black-yukoku)` (14%) |
| `--muted` / `--accent` | `var(--black-kenpo)` (12%) |
| `--foreground` family | `0 0% 98%` |
| `--muted-foreground` | `0 0% 70%` |
| `--primary` | `0 0% 98%` |
| `--primary-foreground` | `0 0% 9%` |
| `--destructive` | `0 62% 30%` |
| `--border` / `--input` | `var(--black-kurogane)` (17%) |
| `--ring` | `0 0% 83%` |

No-JS fallback: `@media (prefers-color-scheme: dark)` when neither `.dark` nor `.light` is on `html`.

### Pastels (12 × 7)

Hues: amber, orange, red, rose, purple, lavender, indigo, blue, cyan, teal, green, lime.

Steps per hue: `bg`, `subtle`, `border`, `mid`, `fg`, `fg-bold`, `bright`.

Light pattern (amber example):

```
bg 45 40% 96%
subtle 45 35% 93%
border 45 28% 86%
mid 45 25% 75%
fg 45 30% 45%
fg-bold 45 35% 35%
bright 45 45% 55%
```

Dark pattern: lower saturation, lifted lightness on foregrounds; badges should use `subtle` (bg) + `bright` (text).

Tailwind maps under `pastel.<hue>.{DEFAULT,bg,subtle,border,mid,fg,fg-bold,bright,...}` in `tailwind.config.js`.

**Use:** status chips, tracking labels, notices. **Do not** recolor the whole site with pastels.

### Alternate themes

| Theme | File | Character |
|-------|------|-----------|
| Default | `default.css` | 100 Whites / 100 Blacks |
| Solarized | `solarized.css` | Ethan Schoonover sixteen-color palette |
| Plan 9 | `plan9.css` | acme/rio creams and cyans, Bell Labs heritage |

Barrel: `src/app/styles/themes/index.css`. Default is the brand; alternates are opt-in personality modes.

## Typography

### Loaded faces (`layout.tsx`)

| Variable | Files | Role |
|----------|-------|------|
| `--font-sans` | IBM Plex Sans Regular/Bold | UI, navigation, body class on `<body>` |
| `--font-serif` | Noto Serif variable + italic | Content prose, headings in content |
| `--font-mono` | Go Mono Regular/Bold | Code, kbd, operational IDs |

Body class: `font-sans antialiased` plus the three CSS variables.

### Content prose (`.content`, `.essays-content`)

Source: `src/app/styles/core/content.css`.

| Property | Value |
|----------|-------|
| Family | `var(--font-serif)`, Noto Serif, Georgia, serif |
| Size | **13px** |
| Weight | 500 |
| Line height | 1.55 |
| Features | kern, liga, calt |
| Smoothing | antialiased |
| Hyphens | auto |
| Layout | `display: flow-root` (contains floats / wiki cards) |

#### Headings (serif, weight 600, tracking -0.02em)

| Level | Size | Underline |
|-------|------|-----------|
| h1 | 1.625rem (26px) | 1px solid foreground/25% |
| h2 | 1.5rem (24px) | foreground/15% |
| h3 | 1.25rem (20px) | foreground/10% |
| h4 | 1.125rem (18px) | none |

Margins: `margin-top: 2rem`, `margin-bottom: 0.5rem`, `scroll-margin-top: 5rem`. First heading loses top margin.

#### Paragraphs

- `text-align: justify`, `text-align-last: left`
- Margin bottom 12px
- Empty paragraphs hidden

#### Blockquotes (gwern-style nesting)

- Left border 3px, no fill background at top level
- Nested borders darken (0.2 → 0.35 → 0.5 of foreground)
- Font size 0.95em, muted foreground

#### Links in content

- Color inherits foreground (not system blue in content CSS)
- No underline by default; hover opacity 0.7
- (Global `--link` still used where components opt into it)

#### Lists

- Circle / disc alternating by nest depth for ul
- Decimal for ol
- Item spacing 0.5rem

#### Code

- Inline: mono, muted bg, pad 0.15em 0.4em, **radius 0**, nowrap
- Pre: mono 13px, border, muted/50 bg, radius 0, pad 1rem

#### Tables

- Full width, collapsed borders, cell pad 0.75rem
- Header muted/50 bg
- Even rows muted/20
- Wiki cards opt out of generic table chrome

#### Other prose elements

| Element | Treatment |
|---------|-----------|
| Images | block, max-width 100%, radius 4px, centered |
| Figcaption | 0.9em, muted, italic, centered |
| HR | 1px border color, 2rem vertical margin |
| Footnotes | top border, smaller type |
| kbd | mono, muted bg, border, inset shadow |
| strong | 600 |
| Dropcap images | floated left with tight margins (globals) |

### Book content (`.book-content`)

Larger serif reading path: 1.125rem, line-height 1.75, max-width 65ch, dotted underlines on links.

### UI type

Sans (IBM Plex) for nav, buttons, forms, tracking chrome. Keep peer labels equal size; never enlarge one metric because its string is longer.

## Surfaces, borders, radius

- Page is normally one continuous canvas (`background`).
- Earn borders for tables, code, inputs, and true grouping.
- Default radius `0.5rem`; content code and many chrome pieces prefer **square**.
- Scrollbars: 8px, **square** thumb corners, muted-foreground wash.
- Avoid nested cards. Avoid glass. Avoid decorative shadows.

## Components inventory (current patterns)

### Content lists / tables

Two table traditions:

1. **UI primitive** `components/ui/table.tsx` — shadcn-like bare table.
2. **Content table** `components/content/table.tsx` — variants `default` | `minimal`, `YYYY.MM.DD` dates, row hover `secondary/50`, optional sound ticks, clickable rows.

Prefer consolidating new list UIs onto one of these rather than a third.

### Cards

Many card types exist across home, tracking, 404 cards, media. Shared rules:

- Quiet borders or none
- Pastel only for rarity/status
- No icon-in-colored-tile decoration as default

### Hover / tooltip

CSS-only hover cards and tooltips in `globals.css` (not Radix) for lighter JS: `.hover-card-root`, `.tooltip-root`.

### Tracking routes

Per-domain CSS under `src/app/(tracking)/*/*.css` (film, music, anime, games, sports, globe, manga, cartoons, tv). These may add local density rules but should still consume semantic tokens.

## Motion

- Prefer CSS transitions 0.15s ease for opacity/color.
- Flip cards exist (`.card-3d`) with 0.6s rotateY for intentional play surfaces.
- No autoplay marquees.
- Theme changes: `disableTransitionOnChange` on provider to avoid flash painting.

## Sound

Some interactive lists play tick/click sounds on hover/click. Keep optional and quiet; never require sound for comprehension.

## Date and metadata conventions

- Display dates often as **`YYYY.MM.DD`** in content tables.
- Metadata strips vary (audit found multiple header/footer patterns). New work should pick the dominant listing pattern on that route family and match it.
- Importance / certainty / status scales live in taxonomy docs (`.agents/docs/taxonomy.md`), not as rainbow badges without meaning.

## URLs and content identity

- Sexy URLs (`/<slug>`) are canonical for content items (see `~/.claude/docs/krisyotam-urls.md`).
- Use `getCanonicalContentUrl` server-side; consume `item.url` client-side.
- Do not hardcode `/<type>/<category>/<slug>` for permalinks in new UI.

## Images and CDN

| Type | Server path | Public URL |
|------|-------------|------------|
| People | `/mnt/storage/cdn/images/people/<type>/` | `https://krisyotam.com/cdn/images/people/...` |
| General | `/mnt/storage/cdn/images/` | `https://krisyotam.com/cdn/images/...` |

Never put content photography into `public/images/` in the repo.

## Accessibility

- Semantic landmarks, ordered headings, skip patterns where overlays exist.
- Focus rings via `--ring`.
- Keyboard: Escape closes search overlay; Enter submits.
- Do not rely on pastel color alone for status; keep text labels.
- `scrollbar-gutter: stable` to avoid layout shift.

## Anti-patterns (site-specific)

Do not ship:

- A second neutral palette that is "almost" 100 Whites but tinted warm/cool
- Google Fonts runtime loads for core faces (use local `next/font`)
- Content body in sans when the route uses `.content`
- Justified text on very wide measures without a shell
- Rounding every pre/code block with large radii
- Replacing content link inheritance with loud underlines site-wide without care
- New client chart libraries without performance review
- `force-dynamic` on pages that can be static

## Agent checklist before shipping UI

1. Which shell: content, home, or wide?
2. Light and dark both checked against semantic tokens?
3. Prose using `.content` rules if it is article body?
4. Any new color beyond neutrals + existing pastels?
5. Bundle: new client dependency justified?
6. Dates and metadata match nearby pages?
7. Updated this doc if the system actually changed?

## Related files

| Path | Why |
|------|-----|
| `src/app/styles/themes/default.css` | Token source of truth |
| `src/app/styles/core/content.css` | Prose system |
| `src/app/globals.css` | Shells, scrollbars, utilities |
| `tailwind.config.js` | Tailwind token bridge |
| `.doc/audit/design-audit.md` | Component inconsistency ledger |
| `../design.md` | Shared brand kit target |
