# LWServers (lwservers.com/capitol) — Style Reference

Extracted live from the site 2026-07-26 (dark theme active, `data-theme=dark` on `<html>`).
Use these tokens and recipes when building new PSDRoster features in this style.

## Core identity

- **Two-font system**: `Outfit` (UI text, names, headings) + `JetBrains Mono` (ALL data: numbers, labels, chips, eyebrows). The mono-for-data rule is the strongest signature of the look.
- **Dark glassmorphism**: near-black blue-tinted backgrounds, translucent panels with `backdrop-filter: blur()`, 1px white-alpha borders.
- **Tinted-chip formula**: every colored element uses the same recipe — `color: <tone>`, `background: <tone> at ~12-18% alpha`, `border: 1px solid <tone> at ~22-25% alpha`. Applied to chips, badges, avatars, icon tiles, CTAs.
- **Neon green accent** (`#00e87a`) for positive/live/primary; **amber** (`#f59e0b`) as the secondary "gold" tone (used heavily on the capitol page); red/blue/purple for status tiers.

## Design tokens (dark theme)

```css
:root[data-theme="dark"] {
  /* Backgrounds — blue-black ramp */
  --bg-root: #080a0f;
  --bg-1: #0d1117;          /* card/panel base */
  --bg-2: #131920;
  --bg-3: #1a2230;          /* raised elements, inactive pills */

  /* Glass */
  --glass-bg: rgba(13,17,23,.65);
  --glass-bg-2: rgba(19,25,32,.72);
  --glass-bg-3: rgba(255,255,255,.04);
  --glass-border: rgba(255,255,255,.08);
  --glass-border-2: rgba(255,255,255,.14);
  --glass-shine: rgba(255,255,255,.06);
  --blur-sm: blur(8px); --blur-md: blur(16px); --blur-lg: blur(24px);

  /* Accent (neon green) */
  --accent: #00e87a; --accent-2: #00c466;
  --accent-glow: rgba(0,232,122,.18);
  --accent-border: rgba(0,232,122,.22);

  /* Tones */
  --amber: #f59e0b; --amber-glow: rgba(245,158,11,.12); --amber-border: rgba(245,158,11,.25);
  --red:   #f87171; --red-glow: rgba(248,113,113,.12);  --red-border: rgba(248,113,113,.25);
  --blue:  #60a5fa; --blue-glow: rgba(96,165,250,.12);  --blue-border: rgba(96,165,250,.25);

  /* Tier scale (power tiers) */
  --tier-mega: #f87171; --tier-whale: #c084fc; --tier-shark: #60a5fa;
  --tier-piranha: #00e87a; --tier-shrimp: #6b7280;

  /* Text */
  --text-1: #eaecf0;  /* primary */
  --text-2: #8b95a1;  /* secondary */
  --text-3: #4b5563;  /* muted/meta */

  /* Borders */
  --border-1: rgba(255,255,255,.07);
  --border-2: rgba(255,255,255,.12);
  --border-3: rgba(255,255,255,.18);

  /* Type */
  --font-ui: 'Outfit', system-ui, sans-serif;
  --font-data: 'JetBrains Mono', 'Fira Code', monospace;

  /* Shadows */
  --shadow-sm: 0 1px 3px rgba(0,0,0,.4);
  --shadow-md: 0 4px 16px rgba(0,0,0,.5);
  --shadow-lg: 0 8px 32px rgba(0,0,0,.6);
  --shadow-xl: 0 16px 48px rgba(0,0,0,.7);
  --shadow-accent: 0 0 24px rgba(0,232,122,.2);

  --hover-overlay: rgba(255,255,255,.04);
  --hover-overlay-2: rgba(255,255,255,.07);
}
```

### Light theme ("parchment") — for reference

Warm paper palette: `--bg-root #d8c39a`, panels `#ece1c6 → #fbf6e6`, text sepia
(`#2a1d0c` / `#5c4528` / `#8c7651`), borders `rgba(60,38,12,.09-.25)`, accent forest
green `#3d8a32`, amber `#d49616`, red `#c44a3e`, blue `#3d92b8`. Shadows tinted brown
`rgba(80,50,10,…)`. Same structure, swapped values — theme via `data-theme` attribute.

### Spacing / radii / motion (theme-independent)

```css
--sp-1..10: 4 8 12 16 20 24 32 40px;
--r-sm: 6px; --r-md: 10px; --r-lg: 14px; --r-xl: 20px; --r-2xl: 28px;
--nav-h: 64px; --sidebar-w: 220px; --content-max: 1100px;
--page-px: 20px; --page-px-desk: 32px;
--ease: cubic-bezier(0.16, 1, 0.3, 1);   /* springy ease-out */
--ease-in: cubic-bezier(0.4, 0, 1, 1);
--t-fast: 120ms; --t-base: 200ms; --t-slow: 350ms;
```

Base font-size is **15px** on body.

## Component recipes (measured from live site)

### Eyebrow / section kicker
Mono, 10px, weight 600, uppercase, letter-spacing 1.4px, tone color (amber on capitol
page), margin-bottom ~7px. Sits above a 28px Outfit-700 h1 with letter-spacing -1%
and a soft text-shadow `0 2px 18px rgba(0,0,0,.45)`.

### Hero header
Radial amber glow from top + fade-to-transparent linear gradient:
`radial-gradient(120% 90% at 50% -10%, rgba(245,158,11,.12), transparent 62%), linear-gradient(#0d1117, transparent 85%)`;
bottom corners rounded 24px.

### Stat tile (`.cap-tile`)
Glass panel: `background: var(--glass-bg)`, `border: 1px solid var(--glass-border)`,
`border-radius: 16px`, `padding: 16px`, `backdrop-filter: var(--blur-sm)`.
Number: mono 24px/700, letter-spacing -1%, `--text-1`. Label beneath: mono ~9-10px
uppercase, `--text-2`/`--text-3`.

### List row (`.cap-row`)
`display:flex; align-items:center; gap:10px`, `background: var(--bg-1)`,
`border: 1px solid var(--border-1)`, `border-radius: 14px`, `padding: 10px 12px`.
Anatomy left→right:
- **ID box** (`.cap-wz`): mono 12px/700, `--text-2` on `--bg-3`, radius 9px, 44px wide, centered.
- **Avatar** (`.cap-ava`): 30px circle, tinted-chip formula (tone bg 12% + tone border 25% + tone-colored initial, Outfit 13px/700).
- **Main**: primary line Outfit 15px/600 `--text-1`; secondary line mono 12px/500 `--text-3`, flex gap 6px, margin-top 3px.
- **Status chip** (`.cap-chip`): mono 9.5px/700 uppercase, letter-spacing .06em, radius 999px, padding 3px 7px, tinted-chip formula (green: `--accent` on `--accent-glow` + `--accent-border`).
- **Value** (right): mono 16px/800, letter-spacing -1%, `--text-1`; unit below in muted.
Rows carry a tone class (`tone-green`, etc.) tinting chip + avatar together.

### Status pill (`.pill-live`)
Mono 10px uppercase letter-spacing 1px, radius 6px, padding 4px 10px, flex+gap 5px
with a small dot; green at 8% bg / 22% border.

### Badge (small, e.g. "PRO")
Mono 8.5px/700 uppercase, letter-spacing .06em, radius 5px, padding 3px 6px, amber
tinted-chip formula.

### Segmented filter pills (`.cap-seg` / `.cap-cat`)
Radius 999px. Inactive: `--bg-3` bg, `--border-2` border, `--text-1`, Outfit/mono
12-12.5px/600, padding 8-9px 12-14px. **Active: solid tone fill** (amber `#f59e0b`)
with dark text (`#2a1c00`) — full-contrast inversion, not just a tint.

### CTA pill (`.cap-cta`)
Mono 12.5px/700, letter-spacing .03em, radius 999px, padding 9px 18px, tinted-chip
formula (amber), flex + gap 8px, often with emoji/arrow.

### Tool card (`.cap-tool`)
`--bg-1` bg, `--border-1` border, radius 14px, padding 14px, flex column gap 3px.
Leading 34px icon tile: radius 10px, tinted-chip formula. Name Outfit, sub-line muted.

## How this maps to current PSDRoster

Current index.html palette: `--bg #1e2a3a, --panel #2b3a4f, --row #c9d4e3 (light rows!),
--accent #6cc24a, --gold #f2c14e, --silver, --bronze, --tbc #8a4646`.
Rough correspondences if restyling: `--bg→--bg-root`, `--panel→--bg-1`,
`--accent #6cc24a→#00e87a`, `--gold→--amber`. The big divergences: LWServers keeps rows
DARK (no light row backgrounds), uses mono for all data, and uses the tinted-chip
formula instead of solid fills.

## Font loading

```html
<link href="https://fonts.googleapis.com/css2?family=Outfit:wght@400;600;700;800&family=JetBrains+Mono:wght@500;600;700;800&display=swap" rel="stylesheet">
```
