# Assets reference (private — not committed)

This folder is a snapshot of every brand asset used by KODE OS, collected from
both `assets/` and `kode-os-ui/src/assets/` so you can grab them without
hunting through two repos. **Safe to delete** — it's gitignored at root and
nothing in the codebase points here.

## Contents

### `brand/`
| File | Where it's used | Notes |
|---|---|---|
| `logo.svg` | Horizontal wordmark — wizard, About page, BrandBar | Full "KODE NAS" lockup. SVG fills with `currentColor` (white via CSS filter trick when shown on dark) |
| `logo.png` | Fallback raster — older browsers, social cards | Equivalent to `logo.svg` at 2× |
| `logo-mark.svg` | Just the K symbol — favicons, app launcher tile | Square, 1:1 aspect |
| `logo-light.png`, `logo-dark.png` | Pre-rendered raster versions for places where the SVG colour filter can't apply | Light = white-on-dark, Dark = teal-on-light |

### `favicons/`
| File | Where used |
|---|---|
| `favicon.svg` | Modern browsers (vector, scales cleanly) |
| `favicon.ico` | IE / legacy fallback |
| `favicon-16.png`, `favicon-32.png`, `favicon-64.png` | Browser tabs at different DPRs |
| `apple-touch-icon.png` | iOS home-screen icon when the dashboard is "Add to Home Screen"'d |

### `wallpapers/`
| File | Notes |
|---|---|
| `wallpaper.jpg` | KODE NAS default — misty forest scene |
| `default_wallpaper.jpg`, `wallpaper01.jpg`, `wallpaper02.jpg` | CasaOS upstream wallpapers, kept as fallback options users can pick |

## Branding rules

See [DESIGN.md](../DESIGN.md) for the full guide. Short version:

- Don't recolour the logo. White on dark, teal `#2D5F4E` on light. That's it.
- Don't stretch the logo. Aspect ratio is fixed.
- Min logo size: 24 px tall for the mark, 80 px wide for the wordmark.
- Don't put the logo on a busy photo background without the wallpaper scrim.
