---
name: photos-krisyotam-com-current-design
site: https://photos.krisyotam.com
repo: ~/src/photos.krisyotam.com
status: ground-truth
updated: 2026-08-06
sources:
  - styles/index.css
  - tailwind.config.js
  - pages/index.tsx
  - pages/_app.tsx
  - pages/_document.tsx
  - components/SharedModal.tsx
  - components/Modal.tsx
  - components/Carousel.tsx
  - components/MusicPlayer.tsx
  - components/PageDescription.tsx
  - README.md
---

# photos.krisyotam.com - current design system

Ground truth for the photo archive. The product is a **masonry visual archive** with a profile-card gallery picker, infinite-ish loading, and a high-performance lightbox. The image is the design. Chrome exists only to choose galleries, open photos, and get out of the way.

## Product and brand context

photos.krisyotam.com stores galleries of IRL photos, screenshots, PFPs, architecture, food, astrophotography ambitions, and other visual archives. Images live on the CDN (`krisyotam.com/cdn/pics/`), metadata in Postgres, pages statically generated.

Inherited starting point: Vercel Next.js image gallery template. Heavily customized:

- Profile card + nested gallery drill-down instead of event title cards
- Thousands of images (GPU-conscious lightbox)
- Ambient music player
- Shared neutral theme tokens aligned with the main site

Brand posture: **archive, not social network**. No like buttons, no sticky marketing banners, no AI glow. Quiet tools around loud pictures.

## Priority order

1. Never compromise lightbox performance (GPU, memory, re-render cost).
2. Keep gallery navigation understandable (all → folders → leaves).
3. Keep image edges legible on both themes (`--highlight` inset).
4. Keep chrome muted (`muted-foreground`, small icon buttons).
5. Prefer solid overlays over `backdrop-blur`.
6. Prefer `history.replaceState` over `router.push` inside the lightbox flow.

## Stack and integration

| Layer | Choice |
|-------|--------|
| Framework | Next.js **Pages Router** (not App Router) |
| Styling | Tailwind + small `styles/index.css` token layer |
| Theme | `next-themes`, `attribute="class"`, **defaultTheme dark**, `storageKey="theme"` |
| Images grid | `next/image` with blur placeholder |
| Images lightbox | **native `<img>`** (bypass optimizer pipeline while swapping) |
| Icons | Heroicons outline + inline SVG paths in index |
| Music | Howler + Zustand, dynamically imported `ssr: false` |
| Data | Postgres via `DATABASE_URL`, CDN URLs in rows |

## Authorship shell

### Page structure (index)

1. Optional viewport scale wrapper (see below)
2. **Profile / gallery card** (identity + drillable gallery list + theme/social icons)
3. **Page description** panel (about text or gallery blurb)
4. **Masonry grid** of images
5. IntersectionObserver sentinel for loading more
6. **Footer** with clock (Naperville, IL, `YYYY.MM.DD HH:MM:SS`) and icon links
7. **Modal** when `photoId` present
8. **Music player** floating (global)

### Profile card

- Quiet surface using semantic background/card tokens
- Gallery list paginated (**5 items per page**)
- Parent galleries drill in; back control returns to root list
- Active gallery drives image query (`all` uses SSR initial slice)

### Icon button pattern (shared)

```txt
h-7 w-7 rounded-lg
text-muted-foreground
hover:bg-muted hover:text-foreground
stroke icons 1.5
```

Used for RSS, GitHub, home, theme toggle, etc. No filled brand-color icon tiles.

### Footer clock

- Updates via **direct DOM text mutation** every second
- Must not React re-render the page tree
- Format: `YYYY.MM.DD HH:MM:SS in Naperville, IL` (en-GB time, America/Chicago)

## Viewport scaling

Unlike the main site (which scales gutters, not zoom), photos uses CSS `zoom` for density:

```css
.viewport-scale { zoom: 0.60; }
@media (min-width: 1024px) { zoom: 0.67; }
@media (min-width: 1800px) { zoom: 0.75; }
@media (min-width: 2400px) { zoom: 1.0; }
```

This is intentional for showing more of the masonry field on laptop screens. New layout work must verify zoom does not break modal positioning or hit targets.

## Color

### Tokens (`styles/index.css`)

Light:

| Token | HSL / value | Role |
|-------|-------------|------|
| `--background` | `0 0% 98%` | Page canvas (slightly off-white) |
| `--foreground` | `0 0% 20%` | Text |
| `--card` | `0 0% 99%` | Card surface |
| `--card-foreground` | `0 0% 15%` | Card text |
| `--popover` | `0 0% 97%` | Popovers |
| `--primary` | `0 0% 10%` | Primary |
| `--primary-foreground` | `0 0% 95%` | On primary |
| `--secondary` | `0 0% 90%` | Secondary surface |
| `--muted` | `0 0% 92%` | Muted surface |
| `--muted-foreground` | `0 0% 50%` | Meta text |
| `--accent` | `0 0% 96%` | Accent surface |
| `--destructive` | `0 70% 55%` | Errors |
| `--border` | `0 0% 88%` | Borders |
| `--input` | `0 0% 90%` | Inputs |
| `--ring` | `0 0% 40%` | Focus |
| `--radius` | `0.5rem` | Radius |
| `--highlight` | `rgba(0,0,0,0.22)` | **Image inset edge** |

Dark:

| Token | Value |
|-------|-------|
| `--background` | `0 0% 7%` |
| `--foreground` | `0 0% 98%` |
| `--card` | `0 0% 9%` |
| `--muted` / `--accent` / `--secondary` | ~12% greys |
| `--muted-foreground` | `0 0% 70%` |
| `--border` / `--input` | ~15% |
| `--ring` | `0 0% 83%` |
| `--highlight` | `rgba(255,255,255,0.05)` | Subtle edge on dark |

### Tailwind bridge

```js
colors: {
  background, foreground, card, card-foreground,
  muted, muted-foreground, border, accent
}
boxShadow: {
  highlight: "inset 0 0 0 1px var(--highlight)"
}
```

**`shadow-highlight`** is the standard way to put a 1px inset edge on photos so they do not dissolve into the canvas. Use it on grid tiles and relevant frames.

### Overlay chrome (lightbox)

- Prefer **solid** `bg-black/70` (or equivalent) for controls and scrims
- **Do not** use `backdrop-blur` over thousands of images (GPU thrash, especially AMD/Linux)

No pastel system on this site. Neutrals only.

## Typography

Photos is image-first. Type is system / Tailwind defaults via utility classes:

| Role | Treatment |
|------|-----------|
| Profile name / titles | Semibold small sans utilities |
| Gallery list | Compact rows, muted counts |
| Page description | Readable multi-paragraph prose; links allowed in about text |
| Footer | Small muted mono-ish clock string |
| Modal UI | Icon-led; minimal labels |

There is no Noto Serif content pipeline here. Do not import main-site essay CSS.

## Layout

### Masonry grid

- CSS columns / masonry-style flow of image links
- Initial mount budget: **`INITIAL_VISIBLE = 60`**, bump **`PAGE_BUMP = 60`**
- SSR/all-gallery first slice: **`INITIAL_ALL_SLICE = 240`** server images available; client still mounts progressively
- Sentinel `IntersectionObserver` with **`rootMargin: 1500px`** to prefetch/mount ahead
- Per-gallery fetch: `/api/gallery?slug=&offset=&limit=240` with in-flight token to ignore stale responses

### Gallery navigation model

```
all
 └─ parent (has subs) → drill-down list of children
 └─ leaf → images for that category
```

- URL: `/?gallery=slug` with path-looking as `/{slug}` via shallow routing
- `all` → `/`
- Drill state tracked separately from active slug when viewing a parent folder

### Aspect-ratio breakpoints (Tailwind screens)

```js
narrow: max-aspect-ratio 3/2
wide: min-aspect-ratio 3/2
taller-than-854: min-height 854px
```

Used for layout shifts where portrait vs landscape monitors matter.

### Page description

`PageDescription` shows title + body:

| Gallery | Body source |
|---------|-------------|
| `all` | Long-form `ABOUT_DESCRIPTION` constant in `index.tsx` |
| Parent with subs | Template listing child gallery names |
| Leaf with DB blurb | `gallery_descriptions` table |
| Leaf without blurb | `"{name} - {n} images."` |

## Lightbox / modal system

### Architecture

| Piece | Role |
|-------|------|
| `Modal.tsx` | Dialog wrapper, keyboard nav, URL sync |
| `SharedModal.tsx` | Image stage: zoom, pan, filmstrip, download/open |
| `Carousel.tsx` | Full-page direct photo route `/p/[photoId]` |

### Critical performance rules (do not violate)

1. **URL updates use `window.history.replaceState`**, not `router.push`, when scrubbing photos from the index grid. `router.push` re-renders thousands of `Image` nodes and can hard-crash GPUs (green screen).
2. **No framer-motion / AnimatePresence** in the lightbox. Exit animations stacked duplicate images in the DOM. Use `key` remount + simple CSS for filmstrip.
3. **Native `<img>` in lightbox**, `next/image` in grid only.
4. **Solid overlays**, not backdrop-filter blur.
5. Filmstrip window: about **±15** images around current index (not the full catalog).

### Interaction

| Input | Behavior |
|-------|----------|
| Arrow keys / buttons | Prev/next photo |
| Wheel on image | Zoom 0.5×–5× in 0.15 steps |
| Pointer drag | Pan when zoom > 1 |
| Swipe | `react-swipeable` handlers |
| Escape / close | Exit modal, restore scroll position to last viewed thumb when possible |
| Download / open external | Explicit icon actions |

Zoom/pan reset on photo index change.

### Video

URLs ending in common video extensions render as video in the modal path (`mp4`, `webm`, `mov`, …).

## Music player

- Provider + player dynamically imported, `ssr: false`
- Tracks from DB `music` table passed via page props
- Ambient accompaniment; must not block image interaction
- Keep UI compact, theme-aware, non-modal by default

## Motion

- Theme toggle: instant class swap via next-themes
- Grid: progressive mount, no enter animations required
- Lightbox: instant image swap
- Filmstrip: light CSS transitions only
- Hover on icon buttons: color/background only

Default to stillness. Delight is a sharp image and fast next/prev, not choreography.

## Loading and placeholders

- `BLUR_DATA_URL` constant for `next/image` blur
- Safari lazy-load quirk: `clip-path: inset(0.6px)` on lazy images under Apple font support query
- Gallery switch shows loading state without blanking the whole shell carelessly

## Accessibility

- Icon buttons need `aria-label` (pattern already used)
- Modal should manage focus and keyboard nav
- Theme toggle available in chrome
- Do not rely on color alone for selected gallery; use explicit active styles
- Hit targets for filmstrip and arrows must remain usable at `.viewport-scale` zoom levels

## Content and CDN workflow (design-adjacent)

Adding images is an ops concern but affects UI counts:

1. Upload to STARGATE `/mnt/storage/cdn/pics/<category>/`
2. Insert row into `images` (url, title, alt, width, height, category)
3. Optional `gallery_descriptions` for leaf text

Slug collisions across categories fail the build. Design of gallery names must stay unique after slugify.

## Anti-patterns

Do not:

- Call `router.push` on every lightbox next/prev from the heavy index page
- Reintroduce framer-motion image transitions in the modal
- Use `backdrop-blur` on overlays above the grid
- Mount all 6000+ images at once without virtualization/progressive mount
- Put decorative gradients behind photos
- Restyle into a social feed with avatars and heart buttons
- Store binaries in the git repo instead of CDN
- Invent a second highlight mechanism that fights `shadow-highlight`

## Agent checklist

1. Did any navigation path reintroduce full Next router transitions over the full grid?
2. Lightbox still native `img` + replaceState?
3. Light and dark `--highlight` still visible on tiles?
4. Progressive mount budgets still in place?
5. New chrome uses muted icon button pattern?
6. Zoom ladder tested for modal/controls?
7. Update this doc if performance rules or tokens change.

## Comparison to sibling sites

| Concern | photos | krisyotam.com | notes |
|---------|--------|---------------|-------|
| Hero medium | Image | Prose | Stacked notes |
| Type | Utilities / system | IBM Plex + Noto Serif + Go Mono | System UI + mono |
| Pastels | None | Full 12×7 system | None |
| Density tool | CSS zoom ladder | Gutter/measure tokens | Fixed 625px panes |
| Dark default | Yes (defaultTheme dark) | System | System / provider |
| Greatest risk | GPU melt on re-render | Bundle bloat / token drift | Breaking sticky stack |

## Related

| Path | Why |
|------|-----|
| `styles/index.css` | Tokens + zoom |
| `pages/index.tsx` | Shell, gallery logic, about copy |
| `components/SharedModal.tsx` | Lightbox behaviors |
| `README.md` | Architecture and performance decisions |
| `../design.md` | Shared brand kit |
