# brand

Kris Yotam brand kit, research references, and per-site design systems.

This repo is the single place for design language across Kris's properties. Agents and humans read these files before inventing UI.

## Layout

```
design.md                 Canonical brand kit (target / shared identity)
research/                 External DESIGN.md sources and analyses
  vercel.md               Official Vercel design.md
  xai.md                  Community xAI design system analysis
  openai.md               OpenAI brand + product surface analysis
current/                  How each live site actually looks and behaves today
  krisyotam-com.md
  notes-krisyotam-com.md
  photos-krisyotam-com.md
```

## Purpose of each layer

| Path | Role |
|------|------|
| `design.md` | The brand kit. Shared tokens, voice, and rules that should converge over time. |
| `research/` | Source material from other brands. Study, do not copy blindly. |
| `current/` | Ground truth for each deployed site, extracted from code. Update when the product UI changes. |

## Sites covered in `current/`

| File | Live site | Source repo |
|------|-----------|-------------|
| `krisyotam-com.md` | https://krisyotam.com | `~/src/krisyotam.com` |
| `notes-krisyotam-com.md` | https://notes.krisyotam.com | `~/src/notes.krisyotam.com` |
| `photos-krisyotam-com.md` | https://photos.krisyotam.com | `~/src/photos.krisyotam.com` |

## How to use

1. For new UI on a specific site, start with that site's file under `current/`.
2. For brand-wide decisions (logo, shared neutrals, writing tone), use root `design.md`.
3. When borrowing structure or density from another product, open the matching file in `research/`.

## Conventions

- Design docs use markdown. Prefer exact tokens (HSL, hex, rem, px) over vibes alone.
- Cite code paths when documenting "current" behavior so the doc stays falsifiable.
- Prefer updating `current/` over inventing a second parallel system in a product repo.

## License

Personal / private use unless stated otherwise. Third-party research files retain their original licensing and trademark notices (see file headers).
