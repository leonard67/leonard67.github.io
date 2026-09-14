# Design System — Structured Grid / Dark Terminal

## 1. Visual Theme & Atmosphere

The site runs two paired themes on the same layout, toggled via the moon/sun icon in the masthead (`data-theme="dark"` on `<html>`, persisted in `localStorage`). Both share the same structure, spacing, and sharp-cornered component language — only color and a couple of texture details change.

- **Light — "Structured Grid":** off-white background, electric-blue accent, hairline grid texture. Reads as rigorous and technical — a blueprint/schematic feel appropriate for an academic profile.
- **Dark — "Dark Terminal":** near-black background, mint-green accent, the same hairline grid inverted. Reads as a terminal/monospace-forward interface — the boldest "futuristic" statement of the two.

**Key characteristics (both themes):**
- Sharp corners everywhere — `border-radius: 0` site-wide (buttons, cards, tables, the profile photo)
- A subtle 64px hairline grid tiled across the page background (`repeating-linear-gradient`, theme-tinted)
- Bracket-corner ("HUD reticle") frame on the profile photo, with a mono `STATUS · ACTIVE` label beneath it
- Nav links in uppercase monospace with letter-spacing; the active page keeps its existing accent-colored underline
- Headings in a display sans (Space Grotesk); body copy stays in IBM Plex Sans; labels/nav/footer/toc in IBM Plex Mono

## 2. Color Palette & Roles

### Light theme ("Structured Grid")
- **Accent / links / base** (`--global-base-color`): `#3457ff`
- **Accent hover**: `#1c3fe0`
- **Accent visited**: `#142fb8`
- **Background** (`--global-bg-color`): `#f7f8fa`
- **Footer background**: `#f0f1f5`
- **Card / code background**: `#ffffff`
- **Border**: `#d7dbe3`
- **Dark border** (stronger dividers): `#b9bfcb`
- **Primary text**: `#0f172a`
- **Secondary text**: `#475569`
- **Grid line texture** (`--global-grid-line-color`): `rgba(15,23,42,0.05)`

### Dark theme ("Dark Terminal")
- **Accent / links / base** (`--global-base-color`): `#3fe6c4`
- **Accent hover**: `#62f0d3`
- **Accent visited**: `#2bbfa1`
- **Background** (`--global-bg-color`): `#0a0d10`
- **Footer background**: `#10151a`
- **Card / code background**: `#10151a`
- **Border**: `#1c2226`
- **Dark border** (stronger dividers): `#10151a`
- **Primary text**: `#f2f5f4`
- **Secondary text**: `#93a1a1`
- **Grid line texture** (`--global-grid-line-color`): `rgba(255,255,255,0.045)`

### Semantic (shared)
- **Danger**: `#ee5f5b`
- **Warning**: `#f89406`
- **Success**: `#149e61` (light) / `#3fe6c4` (dark, mirrors the accent)

## 3. Typography Rules

### Font families
- **Display / headings** (`$header-font-family`): `"Space Grotesk"`, falls back to IBM Plex Sans → Helvetica Neue → Helvetica → Arial → sans-serif
- **Body** (`$global-font-family` / `$sans-serif`): `"IBM Plex Sans"`, falls back to Helvetica Neue → Helvetica → Arial → sans-serif
- **Labels / nav / footer / toc / code** (`$monospace` / `$sans-serif-narrow`): `"IBM Plex Mono"`, falls back to Monaco → Consolas → Lucida Console → monospace

Loaded via Google Fonts in [`_includes/head/custom.html`](_includes/head/custom.html): IBM Plex Sans (400/500/600/700), IBM Plex Mono (400/500), Space Grotesk (500/600).

### Hierarchy
| Role | Font | Notes |
|------|------|-------|
| Page/site title in masthead | Space Grotesk, 600 | normal case, tight tracking |
| H1–H6 (page content) | Space Grotesk | via `$header-font-family` |
| Nav links (non-brand) | IBM Plex Mono | uppercase, `letter-spacing: 0.08em`, `font-size: 0.72em` |
| Body copy | IBM Plex Sans | unchanged from base theme |
| Sidebar sub-headings, footer, TOC | IBM Plex Mono | via `$sans-serif-narrow` |
| Status label under avatar | IBM Plex Mono | uppercase, accent-colored, `font-size: $type-size-8` |

## 4. Component Stylings

### Border radius
`$border-radius: 0` in both theme files — buttons, position/education tables, the TOC panel, and the profile photo frame are all sharp-cornered. (A handful of unrelated 4px/14px/50% radii remain on small pre-existing elements like the mobile hamburger menu and comment avatars — untouched.)

### Profile photo ("bracket frame")
- Square photo (`border-radius: 0` at the `$large` breakpoint), 1px border in the theme border color, 5px inner padding
- Two accent-colored 2px corner brackets (`::before`/`::after` on `.author__avatar`), 16×16px, top-left and bottom-right only — a HUD-style reticle rather than a full frame
- `STATUS · ACTIVE` mono label beneath, accent-colored, shown only at the `$large` breakpoint and up

### Background grid
`body` gets a two-axis `repeating-linear-gradient` (64px cells) using `--global-grid-line-color`, fixed-attachment so it doesn't scroll with content. Same technique in both themes; only the line color/opacity changes.

### Buttons, tables, nav
Existing minimal-mistakes component structure is unchanged — only the color variables, border-radius, and font-family feed through. The active-nav underline (`.masthead__menu-item.selected a`) already used the accent color and needed no structural change.

## 5. Layout Principles

No layout/grid changes from the base theme — spacing scale, breakpoints, and the sidebar/content split are untouched. The two additions are:
- 64px background grid cell size
- 16px bracket-corner size on the avatar, offset -2px outside the photo edge

## 6. Depth & Elevation
- Light: `rgba(15,23,42,0.05) 0px 2px 10px` (flatter than the old Kraken shadow — the aesthetic favors hairlines over soft shadows)
- Dark: `rgba(0,0,0,0.35) 0px 2px 10px`

## 7. Do's and Don'ts

### Do
- Keep corners sharp (`border-radius: 0`) — this is the load-bearing detail that ties "Structured Grid" and "Dark Terminal" together
- Use IBM Plex Mono for anything label-like (nav, status tags, table periods/dates, footer, TOC)
- Use Space Grotesk only for headings/display text, never body copy
- Keep the grid texture subtle — it's a background detail, not a foreground pattern

### Don't
- Don't reintroduce rounded corners or soft drop shadows — that pulls back toward the old Kraken-purple aesthetic this replaced
- Don't add a second accent hue per theme — light stays single-hue blue, dark stays single-hue green
- Don't use monospace for body paragraphs — it's for labels and structural text only

## 8. Responsive Behavior
Breakpoints unchanged from the base theme (`$sidebar-screen-min-width: 1024px`, `$large: 925px`, etc.). The avatar brackets and status label are gated behind the `$large` breakpoint — on mobile the avatar reverts to a plain circular photo with no bracket/status decoration, matching the "no fake chrome at small scales" rule.

## 9. Agent Prompt Guide

### Quick Color Reference
- Light accent: `#3457ff` · Dark accent: `#3fe6c4`
- Light background: `#f7f8fa` · Dark background: `#0a0d10`
- Light text: `#0f172a` · Dark text: `#f2f5f4`
- Border radius: always `0`

### Example Component Prompts
- "Create a position card: 1px solid border (theme border color), 0 radius, `IBM Plex Mono` 11px uppercase accent-colored date label, `Space Grotesk` 16px 600 role title, `IBM Plex Sans` 13px secondary-color institution."
- "Add a status chip: `IBM Plex Mono`, uppercase, `letter-spacing: 0.08em`, accent-colored, no background/border — just colored text."
