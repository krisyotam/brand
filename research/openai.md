<!--
  Sources:
  - https://openai.com/brand/ (official trademark / wordmark guidelines)
  - Public observation of openai.com and ChatGPT product chrome
  - Community DESIGN.md patterns (no official openai.com/design.md at retrieval time)
  - Apps SDK UI notes: https://developers.openai.com/plugins/concepts/ui-guidelines
  Retrieved / compiled: 2026-08-06

  Note: OpenAI does not publish an official root design.md analogous to
  https://vercel.com/design.md. This file is a research synthesis for agents
  studying OpenAI's public visual language. Not affiliated with or endorsed by
  OpenAI. OpenAI, ChatGPT, GPT, and related marks are trademarks of OpenAI.
-->

---
version: alpha
name: openai-brand-surface-analysis
description: Research synthesis of OpenAI's public brand and product surfaces - calm near-monochrome system, OpenAI Sans wordmark discipline, generous white space, editorial restraint, soft greys, rare teal/black depth, system-native product UI for ChatGPT.
---

# OpenAI design research

Act as a careful observer of OpenAI's public brand and product UI. Prefer restraint, clarity, and soft clinical calm over marketing spectacle.

## Brand context

OpenAI presents itself as a research lab and product company at the same time. The marketing site and ChatGPT share a quiet confidence: large readable type, lots of open space, monochrome or near-monochrome palettes, and very little decorative chrome.

Official partner guidelines at [openai.com/brand](https://openai.com/brand/) cover trademarks and wordmark usage, not a full product design system. There is no official `openai.com/design.md` equivalent to Vercel's published skill at time of writing.

Use this file as research input for agents. Do not treat it as a license to imitate OpenAI trademarks in shipping products.

## Priority order (research-derived)

When requirements compete:

1. Preserve factual accuracy and product honesty.
2. Prefer calm hierarchy over decoration.
3. Prefer system neutrals over brand color fields.
4. Prefer typography and spacing over borders and cards.
5. Prefer one clear action over multiple competing CTAs.

## Authorship and marks

### Wordmark

- Primary identity is the **OpenAI wordmark**, set in **OpenAI Sans**.
- The capital **O** is a perfect circle in the official mark.
- Wordmark is the most direct distillation of the visual philosophy (per brand guidelines).
- Do not redraw, stretch, recolor, or place the mark on noisy backgrounds without sufficient contrast.
- Do not use OpenAI marks for third-party product chrome without permission.

### Product names

- **ChatGPT**, **GPT**, and related product names are trademarks.
- In UI copy, product names stay sentence case or official casing; avoid sloganizing them.

## Color

### Marketing / brand surface (observed)

OpenAI marketing surfaces favor:

| Role | Approx value | Notes |
|------|--------------|-------|
| Canvas light | `#ffffff` / near-white | Default marketing canvas |
| Canvas soft | `#f7f7f8` – `#f4f4f4` | Soft panels, sidebars |
| Ink | `#0d0d0d` – `#202123` | Primary text |
| Body muted | `#6e6e80` – `#8e8ea0` | Secondary text; `#8e8ea0` appears often as a mid grey |
| Hairline | `#e5e5e5` – `#ececec` | Dividers |
| Link / accent (product) | soft black or system blue in docs | Marketing rarely uses loud brand color blocks |
| Dark canvas | near-black `#050505` – `#0d0d0d` | Dark product / marketing sections |
| On dark body | `#ececec` | Readable body on dark |

### Semantic (product, approximate)

Product UIs (ChatGPT and related) use soft greys more than saturated brand hues:

- Success / info / warning remain quiet and system-like.
- Destructive red exists for true errors, not decoration.
- Accent green or teal appears sparingly in historical product chrome; current ChatGPT leans greyscale with subtle accents.

### Rules

- Design in monochrome first. Add color only for state, data series, or true brand moments.
- Never use gradient text, glow CTAs, or neon AI clichés.
- Do not wrap every block in a colored card. OpenAI surfaces prefer continuous canvas + spacing.

## Typography

### Faces

| Role | Face | Notes |
|------|------|-------|
| Brand / wordmark | OpenAI Sans | Proprietary; not freely redistributable for third-party brands |
| Marketing UI | OpenAI Sans or close geometric sans | Clean, humanist-geometric |
| Product UI | system stack / product sans | ChatGPT feels system-native |
| Code | monospace (product-dependent) | Inline code and code blocks only |

Fallbacks when OpenAI Sans is unavailable (research only, not for faking the brand):

```
system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif
```

### Hierarchy (approximate marketing scale)

| Role | Size | Weight | Tracking | Use |
|------|------|--------|----------|-----|
| Display | 48–72px | 400–500 | slight negative | Hero claims |
| Title | 32–40px | 500–600 | slight negative | Section titles |
| Heading | 20–24px | 500–600 | normal | Subheads |
| Body | 16–18px | 400 | normal | Reading prose |
| Small / meta | 12–14px | 400–500 | normal or slight positive | Captions, nav |

### Type rules

- Sentence case for headings. Avoid all-caps eyebrows.
- Body line length roughly 60–75 characters.
- Hierarchy comes from size and weight before color.
- Do not invent playful display faces. The brand is editorial, not carnival.

## Spacing and layout

### Rhythm

- Base unit effectively **8px**.
- Common gaps: 8, 12, 16, 24, 32, 48, 64, 96.
- Marketing pages use large vertical section spacing (48–96px) and generous side gutters.
- Product UI is denser: 8–16px internal component gaps, still with calm outer margins.

### Layout principles

- One continuous canvas. Surfaces are rare and earned.
- Content measure for marketing prose is wide but not full-bleed wall-of-text.
- Grids stay simple: single column reading, two-column feature splits, occasional three-up peer cards.
- Avoid dense dashboard chrome on marketing pages.

### Radii

- Soft, modest radii: often **8px** for cards/inputs, **9999px** for true pills (buttons, chips).
- Not sharp-zero like some developer brands; not playful blob radii either.

## Components (observed)

### Buttons

- Primary: filled near-black (light theme) or filled white (dark theme), pill or soft-round.
- Secondary: outline or soft grey fill.
- Text buttons for low emphasis.
- Labels are short verbs. No emoji in default CTAs.

### Inputs

- Light borders, soft focus rings.
- Placeholder text muted.
- Error text in restrained red below the field, not only color change.

### Cards

- Used for product feature clusters and gallery-like grids.
- Soft border or soft shadow, never heavy drop shadows or glassmorphism.
- Peer cards share equal type roles and padding.

### Navigation

- Sparse top nav: logo/wordmark left, few links, account/CTA right.
- Sticky headers stay thin and calm.
- Mobile collapses to a simple menu, not a marketing megamenu of noise.

### Chat product patterns (ChatGPT-class)

- Full-height app shell.
- Conversation column centered with a comfortable measure.
- Soft message bubbles or flush message blocks depending on surface generation.
- Composer fixed at bottom: rounded input, send affordance, model/tool chips when needed.
- Side nav for history: list density medium, muted metadata, clear active state.

## Motion

- Default to stillness.
- Short fades and height transitions for expanding panels (150–250ms).
- No autoplay marquee heroes, typing-cursor gimmicks, or particle fields.
- Respect `prefers-reduced-motion`.

## Voice and copy (UI)

OpenAI product/marketing copy tends toward:

- Direct claims with careful qualification.
- Short sentences.
- Technical literacy without slang theater.
- Avoid hype words: "revolutionary", "magic", "unleash", "game-changing".

For agents writing in an OpenAI-like surface:

- Prefer concrete nouns and active verbs.
- Prefer "Try" / "Learn more" / "Get started" over theatrical CTAs.
- Avoid em dashes as decorative punctuation in marketing sentences if matching Kris's house style (note: OpenAI's own copy sometimes uses them; Kris's house style still bans them for Kris-authored content).

## Anti-patterns (reject)

Do not ship these when aiming for an OpenAI-like calm:

- Purple/pink AI gradients as default brand identity.
- Glassmorphism dashboards.
- Neon glow buttons.
- Giant icon tiles with random multicolor fills.
- Stock "robot handshake" hero imagery.
- Dense SaaS pricing card noise as the first viewport.
- Fake certainty, fake metrics, decorative charts.
- Over-rounded toy UI (12px+ radii everywhere without hierarchy).

## Accessibility

- Meet WCAG AA contrast for body text.
- Visible focus rings on all interactive controls.
- Do not rely on color alone for state.
- Semantic landmarks and ordered headings.
- Native controls preferred over custom widgets without keyboard support.

## How this differs from peer AI brands

| Brand | Signature | OpenAI contrast |
|-------|-----------|-----------------|
| Vercel | Black/white Geist precision, developer-report craft | OpenAI is softer, more editorial/product-human |
| xAI | Near-black canvas, white outline pills, Universal Sans | OpenAI uses more white canvas and softer greys |
| Anthropic/Claude | Warm terracotta accent | OpenAI stays cooler and greyer |

## Agent usage notes

When an agent is told to "design like OpenAI":

1. Start monochrome. Light canvas, dark ink, muted secondary.
2. Use large calm type and large empty space.
3. Prefer one primary filled CTA and quiet secondary actions.
4. Prefer continuous surfaces over card grids.
5. Never paste OpenAI logos into third-party products without rights.
6. Do not claim official endorsement.

## Provenance

| Claim type | Source quality |
|------------|----------------|
| Trademark / wordmark rules | Official brand page |
| Exact product CSS tokens | Not fully public; values above are approximate from public surfaces |
| Full official design.md | **Does not exist** as of 2026-08-06 (unlike Vercel) |

If OpenAI later publishes an official design.md, replace this research file with that source and keep a dated archive copy.
