# Design Plan — Pelican Bike UI Redesign

## Tokens (derived from scene artwork)
- `--sky-day: linear-gradient(#7ec8f7 → #cfeefb)` — from SVG skyDay
- `--sky-night: linear-gradient(#0a1030 → #1c2a55)` — from SVG skyNight
- `--sun: #ffdf5e` — sun circle
- `--moon: #e8ecf5` — moon circle
- `--asphalt: #3d434e` — road rect
- `--lane: #f4f4f4` — lane lines
- `--beak: #f5a623` — pelican beak polygon
- `--bike-teal: #2aa198` — bike frame stroke
- `--cream: #f7f4ec` — pelican body fill
- `--grass: #8fd694` — ground rect
- `--shadow: #22262e` — wheel fill

Palette selection: 5 named UI colors + scene-derived gradients.
- `--ink: #1a1d24` (near-black, readable on both skies)
- `--paper: #fdfcf8` (cream from pelican)
- `--accent: #f5a623` (beak orange)
- `--teal: #2aa198` (bike frame)
- `--asphalt-dark: #2a2d36`

## Type (system font stacks, no webfonts)
- Display / score: `system-ui, -apple-system, "Segoe UI", "PingFang SC", "Microsoft YaHei", sans-serif`; weight 700; letter-spacing `0.05em`; `font-variant-numeric: tabular-nums` so digits don't jitter.
- Body / labels: weight 400, `line-height: 1.6`.
- Overlays: large title weight 800, small keys weight 400.

## Layout
- **HUD**: absolute top-left. Score is hero (largest, tabular, accent color). Distance / dodge / lives / best grouped in a single row pill chip with `backdrop-filter: blur(6px)` and `rgba(0,0,0,0.35)` background. No emoji used as layout — only one sparing decorative emoji in the title. Corner-aligned so it never overlaps the lane scene.
- **Start overlay**: centered flex panel, same pill/chip border-radius logic (`999px` or `24px`) as buttons, dark translucent background (`rgba(10,16,32,0.82)`), no generic gradient wash.
- **Game-over overlay**: same panel treatment. Score / distance / best in clean column (tabular numbers). New-record celebration kept (`🎉`) but only as inline text, not layout.
- **Touch controls**: bottom center, same radius (`50%` = pill buttons), same border (`2px solid rgba(255,255,255,0.55)`), same shadow and `:active` scale feedback. Shared family with start/game-over CTAs.
- **Milestone toast**: positioned at `top: 60px; left: 50%; transform: translateX(-50%)`, short pill (`border-radius: 999px`), same shadow/blur, fades out in 2s, never overlaps HUD because it's lower and transient.
- **Pause tip**: small text `top: 12px; right: 14px`, opacity 0.7, no emoji.

## One memorable bold element
**The milestone toast and HUD score** spend the boldness: large tabular-nums score in beak-orange (`#f5a623`) with a single dark glass chip behind it. Everything else stays quiet: thin borders, muted opacities, no decorative gradients.

## Self-critique vs generic AI defaults
- Cream+terracotta: avoided; cream (`#fdfcf8`) is the pelican body, terracotta is replaced by exact beak `#f5a623`.
- Near-black+one acid accent: ink is `#1a1d24` (derived from night sky), acid accent is replaced by bike teal `#2aa198` for secondary actions, not a random neon.
- Identical rounded cards: panels use `border-radius: 24px`, buttons use `999px` — differentiated shapes, not identical cards.
- Gradient washes as decoration: no decorative gradient washes; the only gradients are the in-game sky gradients (`skyDay`, `skyNight`) which are part of the scene, not overlay decoration. The start/game-over buttons use solid `accent` / `teal` with `box-shadow`, not gradient fills.
- Revised: removed the orange/blue-purple gradient CTAs and replaced them with solid `accent` and `teal` in the same family, because the gradient was a generic default that didn't match the scene palette.

## Final implementation notes (actual build)
- Game HUD: hero score in accent (#f5a623), chip cluster (dist/dodge/lives/best) with tabular-nums; no emoji used as layout.
- Buttons: one family — start (accent), again (teal #2aa198), touch controls (dark glass, same radius/shadow/press feedback, focus-visible outline).
- Overlays: shared translucent panel (`rgba(8,12,30,.84/.86)`); clean score/distance/best column with `.num` tabular spans; new-record celebration kept (`🎉`).
- Milestone toast / pause tip: kept existing behavior; styled to chip system.
- index.html: same tokens (accent button, ink title); same button family; polished active-voice copy.
