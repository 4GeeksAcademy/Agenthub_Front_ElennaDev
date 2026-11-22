---
name: "AgentHub Admin"
description: "Dark-first B2B SaaS administration design system for AgentHub, built around crisp slate surfaces, indigo brand accents, and high-contrast status indicators."
---

## Overview

AgentHub Admin is an internal B2B operations console for managing AI agents, client companies, modular skills, active contracts, and execution error logs. The interface projects technical precision, calm authority, and scannable clarity for internal operators monitoring business metrics and system health.

The visual direction is **Corporate / Modern Dark-First**: deep slate-navy backgrounds (`#13121B` / `#1B1B24`) paired with layered tonal surfaces, subtle 1px borders, and an indigo brand accent (`#4F46E5` / `#8582FF`) that anchors active navigation, primary actions, and informational callouts. Every interactive element prioritizes clarity and restraint—avoiding decorative gradients, glassmorphism, or neon glows.

## Colors

The palette derives from an Indigo (`#4F46E5`) seed and uses Material tonal roles adapted for dark-first operational clarity, alongside semantic status tokens for agent health, contract lifecycle, and error severity.

- **Primary (Indigo):** Anchors the active sidebar item, primary buttons, focus rings, and informational callouts.
- **Surfaces:** Layered slate-navy tones in dark mode (`#0E0D16` lowest, `#1B1B24` sidebar/cards, `#1F1F28` elevated tables/modals) and crisp off-white surfaces in light mode.
- **Status & Metrics:** Semantic colors carry strict meaning—green for active/healthy, red for failing/critical, amber/yellow for mild warnings, orange for medium severity, blue for agent counts and revenue metrics, and purple for discount/coupon loss metrics.

```yaml
colors:
  brand-indigo: "#4F46E5"
  brand-indigo-soft: "#26234D"
  brand-indigo-border: "#3B377A"
  brand-indigo-text: "#C3C0FF"
  accent-blue: "#3B82F6"
  accent-blue-soft: "#172554"
  accent-blue-text: "#93C5FD"
  accent-purple: "#A855F7"
  accent-purple-soft: "#3B0764"
  accent-green: "#22C55E"
  accent-green-soft: "#14532D"
  accent-green-text: "#86EFAC"
  accent-red: "#EF4444"
  accent-red-soft: "#450A0A"
  accent-red-text: "#FCA5A5"
  accent-yellow: "#EAB308"
  accent-yellow-soft: "#422006"
  accent-yellow-text: "#FDE047"
  accent-orange: "#F97316"
  accent-orange-soft: "#431407"
  accent-orange-text: "#FDBA74"
  accent-neutral-soft: "#2A2933"
  accent-neutral-text: "#C7C4D8"
  danger-solid: "#DC2626"
  on-danger-solid: "#FFFFFF"
  terminal-bg: "#09090E"
  terminal-text: "#86EFAC"
  light:
    primary: "#3525CD"
    on-primary: "#FFFFFF"
    primary-container: "#4F46E5"
    on-primary-container: "#FFFFFF"
    secondary: "#58579B"
    on-secondary: "#FFFFFF"
    secondary-container: "#E2DFFF"
    on-secondary-container: "#1D00A5"
    error: "#BA1A1A"
    on-error: "#FFFFFF"
    error-container: "#FFDAD6"
    on-error-container: "#93000A"
    background: "#F8FAFC"
    on-background: "#1B1B24"
    surface: "#FFFFFF"
    surface-container-lowest: "#FFFFFF"
    surface-container-low: "#F5F2FF"
    surface-container: "#F0ECF9"
    surface-container-high: "#EAE6F4"
    surface-container-highest: "#E4E1EE"
    on-surface: "#1B1B24"
    on-surface-variant: "#464555"
    outline: "#777587"
    outline-variant: "#C7C4D8"
    scrim: "#000000"
  dark:
    primary: "#C3C0FF"
    on-primary: "#1D00A5"
    primary-container: "#4F46E5"
    on-primary-container: "#FFFFFF"
    secondary: "#C3C0FF"
    on-secondary: "#2A276A"
    secondary-container: "#2A276A"
    on-secondary-container: "#E2DFFF"
    error: "#FFB4AB"
    on-error: "#690005"
    error-container: "#93000A"
    on-error-container: "#FFDAD6"
    background: "#0E0D16"
    on-background: "#E4E1EE"
    surface: "#1B1B24"
    surface-container-lowest: "#0E0D16"
    surface-container-low: "#16151F"
    surface-container: "#1F1F28"
    surface-container-high: "#2A2933"
    surface-container-highest: "#35343E"
    on-surface: "#E4E1EE"
    on-surface-variant: "#ACA9BC"
    outline: "#918FA1"
    outline-variant: "#302F3E"
    scrim: "#000000"
```

## Typography

The type system pairs **Mulish** for crisp geometric headings and KPI figures with **Instrument Sans** for dense UI tables and form controls, and **Spline Sans Mono** for technical stack traces and fixed-width tabular data.

**Icon System:** All icons MUST be rendered as inline linear `<svg>` elements (`stroke="currentColor"`, `fill="none"`, `stroke-width="1.5"`). Do NOT load or use Material Symbols or external icon fonts.

```yaml
typography:
  display-lg:
    fontFamily: "Mulish"
    fontSize: 36px
    fontWeight: 700
    lineHeight: 44px
    letterSpacing: -0.5px
  display-lg-mobile:
    fontFamily: "Mulish"
    fontSize: 28px
    fontWeight: 700
    lineHeight: 34px
    letterSpacing: -0.25px
  headline-lg:
    fontFamily: "Mulish"
    fontSize: 24px
    fontWeight: 700
    lineHeight: 32px
    letterSpacing: -0.25px
  headline-md:
    fontFamily: "Mulish"
    fontSize: 20px
    fontWeight: 700
    lineHeight: 28px
    letterSpacing: 0px
  title-lg:
    fontFamily: "Mulish"
    fontSize: 18px
    fontWeight: 600
    lineHeight: 24px
    letterSpacing: 0px
  title-md:
    fontFamily: "Instrument Sans"
    fontSize: 15px
    fontWeight: 600
    lineHeight: 22px
    letterSpacing: 0px
  body-md:
    fontFamily: "Instrument Sans"
    fontSize: 14px
    fontWeight: 400
    lineHeight: 20px
    letterSpacing: 0px
  body-sm:
    fontFamily: "Instrument Sans"
    fontSize: 13px
    fontWeight: 400
    lineHeight: 18px
    letterSpacing: 0px
  label-lg:
    fontFamily: "Instrument Sans"
    fontSize: 14px
    fontWeight: 500
    lineHeight: 20px
    letterSpacing: 0px
  label-md:
    fontFamily: "Instrument Sans"
    fontSize: 12px
    fontWeight: 600
    lineHeight: 16px
    letterSpacing: 0.25px
  label-sm:
    fontFamily: "Instrument Sans"
    fontSize: 11px
    fontWeight: 600
    lineHeight: 14px
    letterSpacing: 0.4px
  mono-sm:
    fontFamily: "Spline Sans Mono"
    fontSize: 12px
    fontWeight: 400
    lineHeight: 18px
    letterSpacing: 0px
```

## Layout & Spacing

The application uses a persistent left sidebar (`256px` on desktop, `224px` on tablet) paired with a fluid main content area (`min-h-screen`) and a sticky top bar (`64px` height). Content sits inside a padded workspace container (`24px` to `32px`) with a strict 4px/8px spacing rhythm.

- Dashboard KPI cards use a 2×2 grid (`grid-cols-1 md:grid-cols-2`) with generous `24px` gaps.
- Skills catalog uses a responsive grid (`grid-cols-1 md:grid-cols-2 lg:grid-cols-3`) with `20px` gaps.
- Tables span full width inside bordered surface containers without horizontal page overflow at `768px` or `1280px`.

```yaml
spacing:
  gutter-xs: 4px
  gutter-sm: 8px
  gutter-md: 16px
  gutter-lg: 24px
  gutter-xl: 32px
  section: 32px
  page-margin: 24px
```

## Elevation & Depth

Hierarchy relies on **tonal layering** and **1px hairline borders** (`outline-variant`) rather than heavy drop shadows. Cards, tables, and the sidebar sit on `surface` (`#1B1B24`) against the `background` (`#0E0D16`), while floating dropdown menus and centered modals step up to `surface-container` (`#1F1F28`) with a subtle ambient shadow and a dark scrim (`scrim` at 60% opacity) behind modals. No backdrop-blur is used.

## Shapes

Corners are crisp and modern: cards, tables, and modals use `12px` (`lg`), buttons and dropdown menus use `8px` (`DEFAULT`), skill chips use `6px` (`sm`), and status badges use `9999px` (`full`) pill geometry.

```yaml
rounded:
  sm: 6px
  DEFAULT: 8px
  md: 10px
  lg: 12px
  xl: 16px
  full: 9999px
```

## Motion & Micro-interaction

Transitions are brisk and functional. Hover states on table rows, sidebar links, and buttons resolve in `100ms`. Collapsible skill drawers expand and collapse smoothly over `300ms` with opacity and height/grid transitions, while chevron icons rotate `180deg`.

```yaml
motion:
  state-change:
    duration: 100ms
    delay: 0ms
    timingFunction: ease-out
  drawer-toggle:
    duration: 300ms
    delay: 0ms
    timingFunction: cubic-bezier(0.2, 0, 0, 1)
  surface-enter:
    duration: 200ms
    delay: 0ms
    timingFunction: cubic-bezier(0.2, 0, 0, 1)
```

## Voice & Content Strategy

- **Tone:** Direct, operational, and clear in Spanish (`es`).
- **Capitalisation:** Sentence case across headings, table headers, buttons, and menu items.
- **Terminology:** Client companies are consistently labeled *Empresa* in Usuarios, *Propietario* in Gestión de agentes, and *Cliente* in Contrataciones.
- **Formatting:** Currency uses Spanish euro format with comma decimal and trailing symbol (`540,00 €`), dates use `DD/MM/YY`, and times use 24-hour `HH:mm`.

## Components

Component implementations live as individual HTML sticker sheet files in
`components/`. Each file contains the canonical, styled HTML components with
`<!-- COMPONENT: name -->` comment markers. Read each file before use.

- **Alert** → `@components/alert.html`: `alert-info`
- **Badge** → `@components/badge.html`: `badge-numeric`, `badge-dot`
- **Button** → `@components/button.html`: `button-primary`, `button-icon`, `button-with-icon`
- **Card** → `@components/card.html`: `card-elevated`, `card-action`, `card-interactive`, `card-interactive-2`
- **Dropdown Menu** → `@components/dropdown-menu.html`: `dropdown-menu-standard`, `dropdown-menu-with-icon`, `dropdown-menu-with-submenu`
- **Modal** → `@components/modal.html`: `modal-standard`, `modal-alert`, `modal-drawer`
- **Table** → `@components/table.html`: `table-standard`, `table-interactive`, `table-selectable`
