---
name: kris-yotam-brand-kit
description: Shared Kris Yotam brand kit. Target identity for krisyotam.com properties, writing surfaces, and new product UI. Site-specific ground truth lives in current/; research references live in research/.
version: 0.1.0
updated: 2026-08-06
---

# Kris Yotam brand kit

This is the **canonical brand kit** for Kris Yotam's work. It describes the shared identity that sites should converge toward. For how each live site operates **today**, read the matching file under `current/`.

## Brand statement

Kris Yotam is a serious generalist writing and shipping on the open web. The brand is paper, ink, restraint, and precision. Not startup hype. Not pastel SaaS. Not neon AI cosplay.

Influences that actually matter:

- **Suckless / Unix** clarity: small surfaces, no ceremony.
- **Kenya Hara "100 Whites"**: pure neutral greys, matte, paper-like.
- **Editorial web** (gwern-adjacent density when the content earns it; Andy Matuschak-style stacked notes for knowledge work).
- **iOS system link colors** for hyperlinks (blue / visited purple), not brand purple CTAs.

## Reader avatar

The Serious Generalist: intellectually curious, polymathic, autodidactic. Philosophy, mathematics, literary fiction, film, history, political economy. Expects restraint, precision, intentionality, and clear epistemic boundaries. Archives outlast opinions.

## Priority order

When requirements compete:

1. Preserve facts, sources, and epistemic honesty.
2. Preserve readability of long-form content.
3. Keep the shell quiet so the work is loud.
4. Prefer spacing and type hierarchy over borders and cards.
5. Prefer fewer features and fewer dependencies.
6. Match the local site's `current/` doc when diverging from this kit is intentional.

## Color

### Philosophy

**100 Whites · 100 Blacks.** Pure neutral (`0° 0%` HSL). Matte. Zen. Surfaces named after Japanese whites and blacks in the main site theme system.

Color is scarce. Pastels exist only for badges, status, and rare semantic markers. They are never the page identity.

### Shared semantic neutrals (target)

Values expressed as HSL components (no `hsl()` wrapper) so they plug into Tailwind-style tokens.

#### Light mode

| Token | HSL | Role |
|-------|-----|------|
| `--background` | `0 0% 100%` | Page canvas |
| `--foreground` | `0 0% 20%` | Primary text (matte charcoal) |
| `--card` | `0 0% 99%` | Slightly off-white surface |
| `--muted` | `0 0% 92%` | Quiet surface |
| `--muted-foreground` | `0 0% 50%` | Secondary text |
| `--border` | `0 0% 88%` | Hairlines |
| `--primary` | `0 0% 10%` | Primary action fill |
| `--primary-foreground` | `0 0% 95%` | On primary |
| `--destructive` | `0 70% 55%` | True errors only |

#### Dark mode

| Token | HSL | Role |
|-------|-----|------|
| `--background` | `0 0% 7%` | Lacquer black (`shikkoku`) |
| `--foreground` | `0 0% 98%` | Primary text |
| `--card` | `0 0% 9%` | Iron |
| `--muted` | `0 0% 12%` | Elevated dark |
| `--muted-foreground` | `0 0% 70%` | Secondary text |
| `--border` | `0 0% 17%` | Black iron |
| `--primary` | `0 0% 98%` | Inverted primary |
| `--primary-foreground` | `0 0% 9%` | On primary |

### Links (shared)

| State | Value |
|-------|-------|
| Default | `rgba(10, 132, 255, 1)` (iOS systemBlue) |
| Visited | `#af52de` (iOS systemPurple) |
| Dark default | `#79a8ff` (notes-site dark adjustment; main site may keep system blue) |
| Dark visited | `#f78fe7` |

### Pastels (use sparingly)

Twelve hues × seven steps (`bg`, `subtle`, `border`, `mid`, `fg`, `fg-bold`, `bright`). Full tables live in `current/krisyotam-com.md`. Rule: badges and status only. Never as full-page brand color.

### Forbidden color moves

- Brand gradients as identity
- Glow / glass / neon AI aesthetics
- Saturated purple SaaS default
- Green bars that mean "good" without a non-color cue
- Rainbow icon tiles

## Typography

### Shared roles

| Role | Preferred face | Fallback |
|------|----------------|----------|
| UI / chrome | IBM Plex Sans or system UI | `system-ui, sans-serif` |
| Long-form body (main site) | Noto Serif | `Georgia, serif` |
| Notes body | `-apple-system` / system UI | system stack |
| Code | Go Mono (main), JetBrains Mono / Geist Mono (notes) | `ui-monospace, Menlo, monospace` |

### Type principles

- Content sites: serif for reading, sans for chrome.
- Tool / notes sites: system sans for everything except code.
- Photos site: chrome stays quiet; the image is the design.
- No decorative display faces for marketing hero moments.
- Sentence case headings. No all-caps eyebrows.
- Justify long-form paragraphs when the measure is narrow enough; keep last line left-aligned.

### Base sizes (guidance)

| Surface | Body size | Line height |
|---------|-----------|-------------|
| Main site prose (`.content`) | 13px | 1.55 |
| Notes panes | 17px / 24px | ~1.4 |
| Photos chrome | system / Tailwind defaults | comfortable |

Main site keeps browser text real and scales **shell gutters**, not root font zoom, for large viewports. Photos still uses a `.viewport-scale` zoom ladder for density (documented in `current/photos-krisyotam-com.md`).

## Layout

### Shell measures (main site)

| Token | Default | Role |
|-------|---------|------|
| `--content-measure` | `42rem` | Reading column |
| `--home-measure` | `48rem` | Home |
| `--wide-measure` | `75rem` | Wide tools |
| `--site-gutter` | `1rem` → `4rem` by breakpoint | Horizontal padding |

### Shared layout rules

- One column reading by default.
- Wide shells only when the content is a table, gallery, or tool.
- Spacing before borders. Borders before cards. Cards last.
- Square or near-square radii for content chrome on main site (`--radius: 0.5rem`, content code often `0`).
- Notes use softer radii on panes and modals (4–12px).
- Photos: full-bleed masonry, profile card as navigation hub.

## Components (shared rules)

### Buttons

- Quiet. Primary is near-black / near-white inversion.
- Prefer text links inside content.
- Icon buttons for chrome (theme toggle, search) with visible hover background, not loud color.

### Tables

- Semantic HTML tables.
- Hairline borders.
- Alternating row wash at low opacity optional.
- Right-align numeric columns when comparing numbers.

### Code

- Mono face only for code and IDs.
- Muted background, hairline border.
- Square corners preferred on main site.

### Images

- CDN-hosted on STARGATE (`krisyotam.com/cdn/...`).
- Never commit site images into product repos.
- Captions muted and centered or left under the figure.

### Theme

- Class-based dark mode (`class="dark"`).
- System preference respected by default.
- Optional alternate themes on main site: Solarized, Plan 9 (opt-in, not default brand).

## Motion

- Default to stillness.
- Transitions 100–200ms, linear or gentle ease.
- No scroll-jacking, no marquee, no parallax heroes.
- Respect `prefers-reduced-motion`.
- Photos lightbox: no framer-motion; instant image swap for GPU safety.

## Voice

Follow `~/.claude/docs/writing.md` for all Kris-authored prose:

- Write like a human. No LLM vocabulary.
- No em dashes as sentence punctuation.
- No triple enumeration autopilot.
- No hedging filler or sycophantic openings.
- Prefer concrete nouns and active verbs.
- Dates prefer `YYYY.MM.DD` in UI metadata where the main site already does.

## Site map under this brand

| Property | Role | Design doc |
|----------|------|------------|
| krisyotam.com | Primary writing / tracking / reference archive | `current/krisyotam-com.md` |
| notes.krisyotam.com | Stacked-pane knowledge base | `current/notes-krisyotam-com.md` |
| photos.krisyotam.com | Visual archive / gallery | `current/photos-krisyotam-com.md` |

These three share neutrals and restraint but **do not** share one CSS package. Convergence happens through this kit and deliberate refactors, not by force-copying components.

## Anti-patterns

- Generic centered SaaS hero + three feature cards
- Gradient text, glow buttons, glass panels
- "AI product" purple defaults
- Nested cards repairing weak hierarchy
- Tiny grey body copy to fake density
- Inventing a second token system beside the live CSS variables
- Storing brand images in git instead of the CDN

## Research corpus

| File | Why it is here |
|------|----------------|
| `research/vercel.md` | Best-in-class official design.md: restraint, evidence, Geist discipline |
| `research/xai.md` | Extreme monochrome product marketing reference |
| `research/openai.md` | Calm product/editorial AI surface reference |

Steal structure and judgment. Do not steal trademarks.

## Maintenance

1. When a site ships a real visual change, update its `current/*.md` in the same effort.
2. When a shared token decision is made (new link color, new serif, new radius policy), update this `design.md` first.
3. Bump `version` in the front matter when the kit changes in a breaking way.
