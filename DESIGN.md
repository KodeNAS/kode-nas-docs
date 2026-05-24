# KODE OS — Design system

Personal reference for the KODE OS visual + interaction language. Not part of the public repo (gitignored).

> If you're a designer mocking up new screens or a developer wondering what colour a button should be: this is the source of truth.

---

## Design idea — one paragraph

KODE OS feels like a **finished home appliance**, not a Linux dashboard. The visual language sits somewhere between Apple Home (translucent tiles, soft drop shadows, generous breathing room) and a high-end Nest device (warm forest greens, calm neutrals, no neon). Every screen has one obvious next action; everything else stays out of the way until you ask for it. The buyer should never see the word "Docker", "Samba", or "container" — those are implementation details, hidden behind names a parent or grandparent already knows ("apps", "file sharing", "your pebble").

---

## Brand identity

| Attribute | Value |
|---|---|
| **Company** | KODE NAS |
| **Product** | pebble v1 |
| **Operating system** | KODE OS |
| **Tagline** | "Your own private cloud, in a box the size of a paperback." |
| **Voice** | Direct, friendly, unintimidating. Sentences short. Active voice. Second person ("your photos") not third ("the user's photos") |
| **Don't say** | NAS, Docker, SMB, TCP, container, FQDN, sudo, daemon |
| **Do say** | "your pebble", "apps", "file sharing", "network", "service" |

---

## Colour

### Primary palette

| Token | Hex | Use |
|---|---|---|
| **Primary** | `#2D5F4E` | Brand colour. Buttons, badges, active links, brand mark |
| **Primary hover** | `#3F7A66` | Hover state on primary buttons + accents |
| **Primary active / pressed** | `#1F4438` | Pressed state, darker accents |
| **Primary light tint** | `rgba(45, 95, 78, 0.12)` | Soft background tint for selected rows, active pills, hover surfaces |
| **Primary glow** | `rgba(45, 95, 78, 0.55)` | Tour spotlight ring, focus halos |

### Surface palette

| Token | Hex / rgba | Use |
|---|---|---|
| **Card background (light)** | `rgba(255, 255, 255, 0.92)` with `backdrop-filter: blur(14px)` | Default tile / modal background |
| **Card border (light)** | `rgba(255, 255, 255, 0.55)` | Subtle 1px tile border |
| **Card backdrop blur** | `blur(14px) saturate(180%)` | Liquid-glass effect on tiles |
| **Wallpaper scrim** | `linear-gradient(180deg, rgba(15,25,30,0.30), rgba(15,25,30,0.55))` | Darkens wallpaper consistently across every route |
| **Dim overlay (modals, tour)** | `rgba(15, 25, 30, 0.62)` | Backdrop dim |

### Text palette

| Token | rgba | Use |
|---|---|---|
| **Text on dark** | `rgba(255, 255, 255, 0.96)` | Body text on wallpaper or dark hero |
| **Text on light primary** | `#1f2937` | Body text on white/card |
| **Text on light secondary** | `rgba(0, 0, 0, 0.72)` | Subdued body, descriptions |
| **Text on light tertiary** | `rgba(0, 0, 0, 0.55)` | Meta, timestamps, hints |
| **Text shadow on dark** | `0 1px 4px rgba(0, 0, 0, 0.55)` | Apply to white text over wallpaper |

### Semantic palette

| State | Background | Text | Use |
|---|---|---|---|
| **Success** | `rgba(45, 95, 78, 0.16)` | `#2d5f4e` | "Online", "Ready", "✓" pills |
| **Warning** | `rgba(196, 127, 0, 0.18)` | `#8a5a00` | Mid-temp warnings, "Heads up" callouts |
| **Danger** | `rgba(168, 50, 57, 0.18)` | `#a83239` | High-temp alerts, errors, destructive actions |
| **Info** | `rgba(45, 95, 78, 0.07)` | `rgba(0, 0, 0, 0.65)` | Tip callouts, footnotes |

### Role badges (family tile)

| Role | Background | Text |
|---|---|---|
| Admin / Root | `#2d5f4e` | `#fff` |
| Editor | `rgba(45, 95, 78, 0.22)` | `#1f4438` |
| Viewer | `rgba(31, 41, 55, 0.12)` | `rgba(31, 41, 55, 0.75)` |

---

## Typography

**Family:** `'IBM Plex Sans', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif`

Loaded via Google Fonts in `public/index.html`. Weights: 300, 400, 500, 600, 700. Don't bring in 800/900 — the brand isn't that loud.

### Scale

| Token | Size | Weight | Line-height | Where |
|---|---|---|---|---|
| **Hero title** | `2.1rem` (33.6 px) | 500 | 1.15 | Welcome screen, splash titles |
| **Display** | `1.5rem` | 500 | 1.2 | Wizard step titles |
| **H1 / page** | `1.25rem` | 500 | 1.25 | Modal headers, panel titles |
| **H2 / section** | `1.0625rem` | 500 | 1.3 | Walkthrough subtitles, popover titles |
| **H3 / tile** | `0.9375rem` | 500, uppercase, `letter-spacing: 0.01em` | 1.3 | Tile titles ("On your pebble", "Network") |
| **Body** | `0.9375rem` | 400 | 1.55 | Default body text |
| **Body small** | `0.875rem` | 400 | 1.5 | Card body, step descriptions |
| **Caption** | `0.8125rem` | 400 | 1.45 | Hints, secondary info |
| **Meta** | `0.75rem` | 400 | 1.4 | Field labels (often uppercase), badge text |
| **Micro / pill** | `0.6875rem` | 600, uppercase, `letter-spacing: 0.04em` | 1 | Role pills, status pills, kickers |

### Headings

- Heading weight: **500** (medium). Never bold.
- Letter-spacing on headings: `-0.01em` (tighten slightly).
- Field labels: `0.75rem`, uppercase, `letter-spacing: 0.04em`, weight 600, colour `rgba(0,0,0,0.62)`.

### Numbers

For anything that ticks (clock, sysinfo percentages, network speeds):
```scss
font-feature-settings: 'tnum' 1;
```
Tabular numerals stop the digits from jittering as values change.

---

## Spacing

Base unit: **`0.25rem` (4 px)**. Everything is a multiple of it.

| Token | rem | px | Use |
|---|---|---|---|
| `xs` | `0.25rem` | 4 | Inline gap inside a pill |
| `sm` | `0.5rem` | 8 | Default flex/gap on tight rows |
| `md` | `0.75rem` | 12 | Field-to-field stack |
| `lg` | `1rem` | 16 | Card body padding |
| `xl` | `1.25rem` | 20 | Tile padding |
| `2xl` | `1.5rem` | 24 | Section spacing |
| `3xl` | `2rem` | 32 | Wizard step margins |

### Radii

| Token | px | Use |
|---|---|---|
| Pill | `999px` | Buttons, pills, badges, family-member chips |
| Card small | `8–10px` | Inputs, small action chips |
| Card medium | `12–16px` | Modal cards, callouts, walkthrough panels |
| Card large | `18–20px` | Tiles, dashboard widgets, SignupModal |
| Wizard shell | `28px` | The first-boot wizard container |

### Shadows

```scss
// Tile / card resting
box-shadow:
  inset 0 1px 0 rgba(255, 255, 255, 0.7),
  0 8px 28px rgba(0, 0, 0, 0.18);

// Modal / popover floating
box-shadow:
  inset 0 1px 0 rgba(255, 255, 255, 0.85),
  0 18px 48px rgba(0, 0, 0, 0.28);

// Wizard shell
box-shadow:
  inset 0 1px 0 rgba(255, 255, 255, 0.10),
  0 24px 60px rgba(0, 0, 0, 0.38);
```

The inset highlight on the top edge is what gives the cards their "lit from above" feel — it's load-bearing for the look, don't drop it when re-styling.

---

## Animations + motion

| Token | Curve / duration | Use |
|---|---|---|
| **Quick state** | `0.15s ease` | Hover, focus, colour transitions |
| **Snap-to-grid** | `0.2s ease` | Drag-and-drop snap |
| **Soft slide** | `0.25–0.3s cubic-bezier(0.34, 1.2, 0.64, 1)` | Tour spotlight, popover repositions |
| **Springy lift** | `0.28s cubic-bezier(0.34, 1.56, 0.64, 1)` | Tile hover, drop, enter |
| **Fade** | `0.2s ease` opacity | Modal/overlay in/out |
| **Step transition** | `0.25s ease` opacity + `translateY(8px)` | Wizard step changes |

**Reduced motion:** the dashboard respects `prefers-reduced-motion: reduce` — animations collapse to instant. New components should follow:

```scss
@media (prefers-reduced-motion: reduce) {
  .my-thing { animation: none !important; transition: none !important; }
}
```

---

## Component patterns

### Liquid-glass tile

Every dashboard widget uses this base:

```scss
background: rgba(245, 247, 250, 0.82);
backdrop-filter: blur(24px) saturate(180%);
-webkit-backdrop-filter: blur(24px) saturate(180%);
border: 1px solid rgba(255, 255, 255, 0.55);
border-radius: 20px;
padding: 1.1rem 1.25rem;
box-shadow:
  inset 0 1px 0 rgba(255, 255, 255, 0.7),
  0 8px 28px rgba(0, 0, 0, 0.18);
```

The `saturate(180%)` is what gives the blur a slightly warmer, richer feel than a flat translucent panel.

### Pill button

```scss
border-radius: 999px;
padding: 0.4rem 1rem;
font-size: 0.8125rem;
font-weight: 500;

// Primary
background: #2d5f4e;
color: #fff;
&:hover { background: #3f7a66; }

// Secondary
background: rgba(0, 0, 0, 0.06);
color: #1f2937;
&:hover { background: rgba(0, 0, 0, 0.10); }
```

### Setup CTA button (wizard / walkthroughs)

```scss
background: linear-gradient(180deg, #3a7762 0%, #2d5f4e 100%);
color: #fff;
padding: 0.85rem 1.25rem;
border-radius: 12px;
border: 1px solid rgba(255, 255, 255, 0.22);
box-shadow:
  inset 0 1px 0 rgba(255, 255, 255, 0.28),
  0 6px 16px rgba(0, 0, 0, 0.22);

&:hover {
  transform: translateY(-1px);
  background: linear-gradient(180deg, #428970 0%, #336d59 100%);
}
```

### Tile header

```html
<header class="tile-header">
  <h2 class="tile-title">{{ Title }}</h2>
  <span class="status-pill is-up">Online</span>
</header>
```

```scss
.tile-header {
  display: flex; align-items: center; gap: 0.5rem;
  margin-bottom: 0.65rem;
  padding-bottom: 0.5rem;
  border-bottom: 1px solid rgba(0, 0, 0, 0.08);
  &.has-gear { padding-right: 36px; }  // conditional in edit mode only
}
.tile-title {
  flex: 1;
  font-size: 0.9375rem; font-weight: 500;
  text-transform: uppercase; letter-spacing: 0.01em;
  color: rgba(31, 41, 55, 0.7);
  margin: 0;
}
```

The status pill (Online, Ready, °C reading, role badge) sits right-aligned because `tile-title` has `flex: 1`. Don't pad-right the header outside edit mode — the pill should sit flush in the top-right corner of the tile.

### Modal-card head

```scss
.modal-card-head {
  display: flex; align-items: center; gap: 0.85rem;
  padding: 1.1rem 1.2rem;
  background: linear-gradient(180deg, rgba(45, 95, 78, 0.06), rgba(255, 255, 255, 0));
  border-bottom: 1px solid rgba(0, 0, 0, 0.06);
}
.head-icon {
  width: 44px; height: 44px; border-radius: 12px;
  background: linear-gradient(135deg, #2d5f4e, #3f7a66);
  color: #fff;
  display: inline-flex; align-items: center; justify-content: center;
}
```

Every primary modal (AddUserModal, SignupModal, AddDeviceWizard step shell) follows this pattern. Don't use Buefy's default modal-card-head.

### Spotlight (tour)

Pure CSS, 4 absolute-positioned dim panels around a clear rectangle:

```scss
.kode-tour-panel {
  position: absolute;
  background: rgba(15, 25, 30, 0.62);
  pointer-events: none;
}
.kode-tour-spotlight {
  position: absolute;
  border-radius: 18px;
  box-shadow:
    inset 0 0 0 2px rgba(255, 255, 255, 0.85),
    0 0 0 4px rgba(45, 95, 78, 0.55),
    0 0 28px rgba(45, 95, 78, 0.55);
}
```

Don't use the giant-box-shadow trick — it gets trapped by transformed stacking contexts on widget tiles.

---

## Layout

### Dashboard grid

- **Three-column default** for Beginner mode. 0.75fr / 1.75fr / 0.7fr weights = clock+weather left, apps middle, family+files right.
- **Sub-columns** allowed (2/3/4 inner columns per outer column) — used by the Advanced and Full-2 templates.
- **6 px dividers** between columns. Drag to resize. Shift+drag snaps to 0.25fr.
- Each tile is a flex-column with `gap: 0.6rem` between internal rows.

### Wizard shell

```scss
.firstboot-shell {
  width: 640px;
  max-height: calc(100vh - 3rem);
  background: rgba(15, 25, 30, 0.32);
  backdrop-filter: blur(22px) saturate(180%);
  border: 1px solid rgba(255, 255, 255, 0.14);
  border-radius: 28px;
  padding: 2.25rem;
  box-shadow:
    inset 0 1px 0 rgba(255, 255, 255, 0.10),
    0 24px 60px rgba(0, 0, 0, 0.38);
}
```

Fixed width 640 px. Scrolls internally if a step (Install progress) is taller than viewport.

### Wallpaper layer

The wallpaper is a `<casa-wallpaper>` component that fills the viewport (`position: fixed`, behind everything). On top of it sits a `.kode-wallpaper-scrim` div with the linear-gradient scrim from the colour palette. Content (router-view) renders above that in a positioned stacking context. **Don't render anything between the scrim and the content layer.**

---

## Iconography

**Icon pack:** custom CasaOS pack, loaded as a webfont. SCSS prefix `casa-`. Pack names verified against the built bundle:

```bash
find build/sysroot/var/lib/casaos/www -name '*.js' \
  -exec grep -hoE 'casa-[a-z][a-z0-9\-]+' {} \; | sort -u
```

Common icons to remember:
- `control-outline` → settings gear
- `dots-vertical-outline` → three-dots menu trigger
- `close-outline` → × close button
- `right-outline` / `left-outline` → chevrons
- `check-outline` → confirm tick
- `alert` → warning / danger
- `information-outline` → info hint
- `internet-outline` → "open this URL" CTAs
- `gallery-outline` → photos / Immich
- `media-outline` → video / Jellyfin
- `folder-outline` → file browser
- `protection-outline` → security / Pi-hole
- `view-dashboard-outline` → smart home / Home Assistant
- `account-outline` → user / account
- `account-multiple` → family / members
- `phone-outline` → phone device step
- `computer-outline` → computer device step

**Wrong icon names render as blank circles silently.** Always verify before shipping.

---

## Empty states

Every list / panel that can be empty has a clear empty state:

```scss
.empty {
  font-size: 0.875rem;
  color: rgba(0, 0, 0, 0.55);
  background: rgba(255, 255, 255, 0.06);
  border: 1px dashed rgba(255, 255, 255, 0.18);
  border-radius: 12px;
  padding: 0.75rem 1rem;
  text-align: center;
}
```

Copy patterns (from the brand voice):
- *"Apps you install will appear here."*
- *"Photos backed up from your phone will appear here."*
- *"No accounts yet."*
- *"Nothing to change — your app selection already matches what's installed."*

Always frame in the **second-person future** ("will appear", "you'll see"). Never an apologetic empty state — the buyer hasn't done anything wrong by not having data yet.

---

## Voice / copy

### Microcopy library

| Context | Copy |
|---|---|
| Welcome heading | "Welcome to your pebble" |
| Welcome subhead | "Your own private cloud, ready in 5 minutes." |
| Setup complete | "You're all set. Your pebble is ready." |
| Primary CTA | "Let's get started" / "Let's go" |
| About tagline | "pebble v1 — made by KODE NAS" |
| Network error | "Can't reach the network. Check the cable to your router." |
| Generic error | "Something went wrong. Try again in a moment." |
| Loading | "Loading…" (with ellipsis) |
| Empty dashboard tile | "Apps you install will appear here." |
| Empty photos | "Photos backed up from your phone will appear here." |
| Empty members | "No accounts yet." |

### Do / Don't

| Do | Don't |
|---|---|
| "your photos" | "the user's photos" |
| "your pebble" | "the device" / "the NAS" |
| "apps" | "containers" / "services" |
| "file sharing" | "Samba" / "SMB" |
| "5 minutes" | "varies" / "depends" |
| "Sign in" | "Authenticate" |
| "Set a password" | "Establish credentials" |
| Sentences. Strong periods. | Long sentences with sub-clauses and parentheses (like this) explaining edge cases inline. |

---

## Accessibility checklist

- Contrast: body text on white ≥ 4.5:1, body text on wallpaper ≥ 4.5:1 (the scrim is sized to guarantee this).
- Focus: every interactive element shows a focus ring (Buefy default + the `:focus-visible` overrides in tile components).
- Keyboard: tab order follows visual order. The dashboard grid is keyboard-reorderable in edit mode (planned: today it's mouse-only).
- Reduced motion: `prefers-reduced-motion` collapses transitions to instant — see Animations section.
- Screen readers: every icon-only button has `aria-label`. Modal headers use `<h3>`. Forms use `<label>`.
- Mobile: every dashboard widget falls back gracefully at < 540 px (single column).

---

## File reference

Where the design tokens actually live in the codebase:

| Token / asset | Location |
|---|---|
| Brand colour, primary palette | `kode-os-ui/src/assets/scss/common/_variables.scss` + scoped `<style>` blocks |
| IBM Plex Sans import | `kode-os-ui/public/index.html` (Google Fonts `<link>`) |
| Wallpaper component | `kode-os-ui/src/components/wallpaper/CasaWallpaper.vue` |
| Wallpaper scrim | `kode-os-ui/src/App.vue` → `.kode-wallpaper-scrim` |
| Tile base styles | Inlined in each `<style scoped>` (FamilyTile, FilesTile, etc.) — pattern is consistent |
| Spotlight + tour styles | `kode-os-ui/src/components/KodeTour.vue` |
| Modal-card head pattern | `kode-os-ui/src/components/beginner/AddUserModal.vue` (canonical example) |

---

## Open design debt

- Tiles are styled inline per-component (FamilyTile, FilesTile, etc.) — should be extracted to a `_tile.scss` partial so changing the base style doesn't mean touching 12 files.
- Two slightly different "modal-card" looks (Buefy default in legacy CasaOS modals vs. the new KODE pattern in AddUserModal / SignupModal / SettingsResetModal). Consolidate during v0.2.
- Light + dark mode: the wallpaper scrim adapts via blur, but text colours assume "dark wallpaper / light card". A real dark mode (dark cards) is v1 roadmap.
- Mobile layout works but isn't actively designed — the dashboard is desktop-first today. Phone-friendly first-boot wizard is v1 roadmap.
