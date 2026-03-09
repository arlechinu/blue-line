# Blue Line Design System

> OPSWAT's shared design system — CSS tokens, components, and icons consumed via GitHub Pages.
> Design System: Blue Line 2.2.2 (Figma source of truth).

**Live docs:** https://vilevich.github.io/blue-line/
**CSS endpoint:** https://vilevich.github.io/blue-line/tokens.css (and components.css, icons.css)

**Scope:** OPSWAT cybersecurity product suite — MetaDefender ecosystem, critical infrastructure protection. Enterprise B2B, security-conscious users, data-dense interfaces.

**What "done right" looks like:**
- Every prototype is pixel-faithful to Blue Line tokens, components, and icon classes.
- Zero invented CSS variables. Zero inline styles. Zero improvised components.
- Prototypes are functional enough to validate interaction patterns with stakeholders.
- Dark mode works via `[data-theme="dark"]` token remapping, not separate stylesheets.
- WCAG 2.2 AA compliance is built in, not bolted on.

---

## 1. FILE STRUCTURE

```
tokens.css      — CSS custom properties, @font-face, dark mode overrides, body base styles
components.css  — All reusable component styles with colocated dark mode overrides
icons.css       — SVG icon classes (inline background-image)
tokens.json     — Structured design tokens (Style Dictionary / Figma Tokens format)
index.html      — Interactive documentation page (self-contained)
fonts/          — Simplon Norm woff2 (400, 500, 700)
CHANGELOG.md    — Version history (Keep a Changelog format)
docs/
  guides/       — Workflow docs: designer-guide, maintainer-guide, developer-guide, ai-prompts
  components/   — Per-component documentation (anatomy, variants, states, accessibility)
  accessibility/ — WCAG overview and checklist
.github/
  ISSUE_TEMPLATE/ — Bug report, component request, accessibility issue templates
  PULL_REQUEST_TEMPLATE.md — PR checklist
  CODEOWNERS    — Review requirements for core files
```

---

## 2. ARCHITECTURE & CONSUMPTION

### Load Order: tokens → components → icons

1. **tokens.css** loads first — defines all CSS custom properties (colors, spacing, typography, radii, shadows) plus `@font-face` declarations and dark mode overrides via `[data-theme="dark"]`
2. **components.css** loads second — references token variables for all UI components. Dark mode overrides are colocated with each component (not in a separate file)
3. **icons.css** loads last — icon classes using inline SVG `background-image`

Products load all three via `<link>` tags. Product-specific styles go in an inline `<style>` block after the DS links.

### CDN Import Pattern

```html
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://vilevich.github.io/blue-line/tokens.css">
<link rel="stylesheet" href="https://vilevich.github.io/blue-line/components.css">
<link rel="stylesheet" href="https://vilevich.github.io/blue-line/icons.css">
```

### Hard Constraints

| Rule | Detail |
|---|---|
| **Token-only styling** | All colors, spacing, typography, shadows, radii must use `var(--token)` from tokens.css |
| **No inline styles** | Zero `style=""` attributes. Everything through classes or CSS variables |
| **No invented variables** | If it's not in tokens.css, it doesn't exist. Flag the gap instead |
| **Component classes only** | Use classes from components.css. No ad-hoc component styling |
| **Icon system** | Use `.icon .icon-{name}` from icons.css. Icons inherit `color` via `currentColor` + CSS masks |
| **Dark mode** | Toggle via `[data-theme="dark"]` on root. All overrides colocated in the CSS files |
| **Reduced motion** | `@media (prefers-reduced-motion: reduce)` is built in — don't break it |
| **High contrast** | `@media (forced-colors: active)` support exists — respect it |

---

## 3. TOKEN INVENTORY (tokens.css)

### 3.1 Fonts
| Token | Value |
|---|---|
| `--font` | `'Roboto', sans-serif` |
| `--font-brand` | `'Simplon Norm', sans-serif` |

### 3.2 Color Palette

**Neutral scale** (12 steps + white):
`--neutral-100` (#f8f9f9) through `--neutral-1200` (#0c121d), `--neutral-white` (#ffffff)

**Blue / Primary** (11 steps):
`--blue-100` (#e8f0ff) through `--blue-1100` (#091b3b)

**Red / Alert** (10 steps):
`--red-100` (#ffdfdb) through `--red-1000` (#7e001d)

**Green** (12 steps):
`--green-100` (#dffff5) through `--green-1200` (#002318)

**Orange** (11 steps):
`--orange-100` (#ffebde) through `--orange-1100` (#551a00)

**Yellow** (12 steps):
`--yellow-100` (#fff8d5) through `--yellow-1200` (#251e00)

**Teal** (12 steps):
`--teal-100` (#e8feff) through `--teal-1200` (#001d1e)

**Purple** (10 steps):
`--purple-100` (#f7e5f8) through `--purple-1000` (#3e1142)

**Constants:**
`--constant-empty` (transparent), `--constant-fade` (white 80%), `--constant-overlay` (neutral 60%)

### 3.3 Semantic Aliases

**Text:**
| Token | Maps to | Use |
|---|---|---|
| `--text-strong` | `--neutral-1200` | Primary text, headings |
| `--text-subtle` | `--neutral-800` | Secondary text, descriptions |
| `--text-muted` | `--neutral-600` | Tertiary text, placeholders |
| `--text-light` | `--neutral-500` | Disabled-look text |
| `--text-white` | `--neutral-white` | Text on dark fills |
| `--text-dark` | `--neutral-1200` | Alias for strong |
| `--text-link` | `--blue-800` | Interactive text links |
| `--text-on-fill` | `--neutral-white` | Text on filled backgrounds |
| `--text-muted-accessible` | `#566479` | WCAG 4.5:1 placeholder text |

**Icons:**
`--icon-strong`, `--icon-subtle`, `--icon-muted`, `--icon-light`, `--icon-extra-light`, `--icon-white`, `--icon-dark`, `--icon-empty`

**Surfaces:**
| Token | Maps to | Use |
|---|---|---|
| `--surface-bg` | `--neutral-100` | Page background |
| `--surface-card` | `--neutral-white` | Card / panel background |
| `--surface-hover` | `--neutral-100` | Hover row highlight |

**Borders:**
`--border-200` (neutral-300), `--border-card` (neutral-200), `--border-bg` (neutral-200)

**Primary / Interaction:**
`--primary` (blue-700), `--primary-hover` (blue-800), `--danger` (red-800), `--danger-hover` (#b0002a)

**Toggle:**
`--toggle-active`, `--toggle-inactive`, `--toggle-border-hover`, `--toggle-fill-disabled`, `--toggle-border-disabled`, `--toggle-thumb-disabled`

**Tab:**
`--tab-bg` (neutral-200), `--tab-active-bg` (#1a2138), `--tab-text` (neutral-800), `--tab-active-text` (white)

**Layout:**
`--sidebar-width` (260px), `--sidebar-collapsed` (60px), `--header-bg` (#1a2138)

### 3.4 Interaction State Tokens
| Token | Value | Use |
|---|---|---|
| `--hover-subtle` | `rgba(0,0,0,0.03)` | Light hover on outline buttons |
| `--hover-default` | `rgba(0,0,0,0.06)` | Standard hover |
| `--focus-ring` | `0 0 0 3px rgba(29,107,252,0.15)` | Focus indicator |

### 3.5 Overlay Tokens
`--overlay-heavy` (0.45 opacity), `--overlay-medium` (0.3 opacity)

### 3.6 Shadows / Elevation
| Token | Use |
|---|---|
| `--shadow-1` | Subtle lift |
| `--shadow-2` | Low cards |
| `--shadow-3` | Medium elevation |
| `--shadow-4` | High elevation |
| `--shadow-card` | Default card shadow |
| `--shadow-dropdown` | Dropdown menus |
| `--shadow-modal` | Modal dialogs |
| `--shadow-panel` | Slide-out panels |
| `--shadow-toast` | Toast notifications |

### 3.7 Typography Scale
| Token | Size | Line-height |
|---|---|---|
| `--size-h1` | 32px | `--lh-h1` 36px |
| `--size-h2` | 24px | `--lh-h2` 27.4px |
| `--size-h3` | 20px | `--lh-h3` 22.8px |
| `--size-h4` | 16px | `--lh-h4` 18.2px |
| `--size-label` | 14px | `--lh-body` 16px |
| `--size-paragraph` | 14px | `--lh-body-paragraph` 18.6px |
| `--size-note` | 12px | `--lh-note` 13.7px / `--lh-note-paragraph` 16px |
| `--size-inset` | 10px | `--lh-inset` 11.4px |

Weights: `--weight-regular` (400), `--weight-medium` (500)

### 3.8 Spacing Scale (Base 4)
| Token | Value |
|---|---|
| `--space-none` | 0px |
| `--space-tiny` | 2px |
| `--space-tight` | 4px |
| `--space-xs` | 8px |
| `--space-sm` | 12px |
| `--space-std` | 20px |
| `--space-double` | 40px |
| `--space-gutter` | 12px |
| `--space-gutter-compact` | 8px |

### 3.9 Corner Radius
| Token | Value | Use |
|---|---|---|
| `--radius-none` | 0px | — |
| `--radius` | 4px | Buttons, cards, fields |
| `--radius-graphics` | 8px | Images, illustrations |
| `--radius-modals` | 12px | Modal dialogs |
| `--radius-circular` | 9999px | Circles, pills |

### 3.10 Breakpoints
| Token | Value | Range |
|---|---|---|
| `--screen-lg` | 1920px | 1461–1920 |
| `--screen-md` | 1440px | 1025–1460 |
| `--screen-sm` | 1024px | 769–1024 |
| `--screen-tb` | 768px | ≤768 |

### 3.11 Element Sizes
`--dot-sm` (6px), `--dot-lg` (8px), `--avatar-sm` (20px), `--avatar-lg` (32px)

### 3.12 Table Tokens
`--table-pad-left` (20px), `--table-pad-right` (12px), `--table-pad-left-mid` (12px), `--table-pad-right-last` (20px)

### 3.13 Feedback Colors
| Token | Hex | Use |
|---|---|---|
| `--feedback-success` | #34a853 | Success dots/icons |
| `--feedback-warning` | #e6a817 | Warning dots/icons |
| `--feedback-warning-text` | #8a6400 | Warning foreground (WCAG) |
| `--feedback-error` | #ea4335 | Error dots/icons |

### 3.14 DS Semantic Color Tokens
Nine semantic families, each with `-100` (bg), `-text` (foreground), `-700` (icon/border), `-fill` (filled bg):

| Family | Purpose |
|---|---|
| `--ds-neutral-*` | Default / neutral state |
| `--ds-inactive-*` | Disabled / inactive |
| `--ds-secure-*` | Security / trust (green) |
| `--ds-success-*` | Success / positive (green) |
| `--ds-accent-*` | Primary accent (blue) |
| `--ds-guide-*` | Guidance / info (purple) |
| `--ds-alert-*` | Alert / danger (red) |
| `--ds-warn-*` | Warning (orange) |
| `--ds-caution-*` | Caution (yellow) |

---

## 4. COMPONENT INVENTORY (components.css)

### 4.1 Buttons
| Class | Type | States |
|---|---|---|
| `.btn-primary` | Filled primary | `:hover`, `:disabled`, `.disabled`, `:focus-visible`, `.btn-loading` |
| `.btn-outline` | Bordered outline | `:hover`, `.danger`, `.btn-loading`, `.skeleton` |
| `.btn-text` | Text-only link button | `:hover`, `.danger`, `.skeleton` |
| `.btn-menu` | Menu/dropdown trigger | `:hover`, `.active`, `.btn-loading`, `.skeleton` |
| `.btn-brand` | Gradient brand CTA | `:hover` |
| `.btn-icon` | 32x32 icon-only | `:hover`, `.btn-loading` |
| `.btn-danger` | Red destructive | `:hover` |
| `.btn-save` | Disabled save (dirty state) | Enabled via `.settings-card.dirty` |
| `.btn-discard` | Discard changes | `:hover`, `:focus-visible` |

**Universal button states:** `:disabled` / `.disabled` (opacity 0.5), `:focus-visible` (blue-300 outline), `.btn-loading` (spinner overlay)

### 4.2 Tabs
| Class | Use |
|---|---|
| `.tabs` | Container (flex, pill bg) |
| `.tab` | Individual tab |
| `.tab.active` | Selected tab |
| `.tab-panel` / `.tab-panel.active` | Tab content panels |

### 4.3 Forms
| Class | Use |
|---|---|
| `.form-row` | Horizontal label + value pair |
| `.form-row--top` | Top-aligned variant |
| `.form-label` | Fixed-width label (266px) |
| `.form-value` | Flex value area |
| `.form-value--inline` | Inline row variant |
| `.help-icon` | 16px help tooltip trigger |
| `.input-field` | Standard text input wrapper |
| `.input-field.error` | Validation error state |
| `.input-field.success` | Validation success state |
| `.input-clear` | Clear button inside input |
| `.input-eye` | Password visibility toggle |
| `.input-error-msg` | Error message text |
| `.input-success-msg` | Success message text |
| `.select-field` | Native select (extends `<select>`) |
| `.input-with-suffix` | Input + suffix label combo |
| `.input-suffix` | Suffix label (e.g., "days") |
| `.file-upload-field` | File upload trigger |
| `.file-upload-text` / `.has-file` | Upload label states |

### 4.4 Radio & Toggle
| Class | Use |
|---|---|
| `.radio-group` | Vertical radio container |
| `.radio-option` | Individual radio item |
| `.radio-option.selected` | Selected state |
| `.radio-option.disabled` | Disabled state |
| `.radio-option.skeleton` | Skeleton loading |
| `.radio-circle` | Visual radio indicator |
| `.radio-label` | Radio text label |
| `.radio-inline` | Horizontal radio layout |
| `.toggle-row` | Toggle + label container |
| `.toggle-switch` | Toggle track |
| `.toggle-switch.active` | On state |
| `.toggle-switch.disabled` | Disabled state |
| `.toggle-switch.skeleton` | Skeleton loading |
| `.toggle-thumb` | Toggle handle |
| `.toggle-label` | Toggle text |

### 4.5 Cards
| Class | Use |
|---|---|
| `.audit-card` | Base card container |
| `.card-header` | Lightweight header (settings) |
| `.card-header h2` | Header title |
| `.card-header-actions` | Header action buttons |
| `.card-header-with-desc` | Header with description |
| `.card-header-desc` | Description text |
| `.card-header-link` | Anchor link in header |
| `.card-body` | Card content padding |
| `.audit-card-header` | Data-table card header |
| `.audit-card-header.with-desc` | With description |
| `.audit-card-header.selected` | Multi-select active (blue bg) |
| `.audit-card-title` | Title + count group |
| `.audit-card-title h3` | Title text |
| `.audit-card-title .count` | Record count |
| `.audit-card-title .count .accent` | Highlighted count |
| `.audit-card-desc` | Header description |
| `.audit-card-actions` | Header action buttons |
| `.audit-card-title-editable` | Inline-editable title |
| `.card-bulk-actions` | Bulk action bar |
| `.card-bulk-divider` | Vertical divider |
| `.card-filter-bar` | Filter chip bar |
| `.card-filter-chip` | Active filter chip (pill) |
| `.column-vis-trigger` | Column visibility dropdown trigger |
| `.card-actions-dropdown` | Card action dropdown trigger |
| `.settings-card.dirty` | Dirty card state (enables save/discard) |

### 4.6 Tables
| Class | Use |
|---|---|
| `.data-table` | Base data table |
| `.data-table.table-fixed` | Fixed-layout big table |
| `.table-scroll` | Horizontal scroll wrapper |
| `.col-cb` | Checkbox column (sticky left) |
| `.col-action` | Action column (sticky right) |
| `.col-pinned` | Pinned name column |
| `.col-user-pinned` | User-pinned columns |
| `.col-actions-cell` | Multi-action cell layout |
| `.audit-table` | Audit-specific table variant |
| `.audit-table-scroll` | Audit scroll wrapper |
| `.filter-icon` / `.filter-icon.active` | Column filter indicator |
| `.col-dropdown` / `.col-dropdown.open` | Column sort/pin dropdown |
| `.col-dropdown-item` / `.active` / `.disabled` | Dropdown options |
| `.row-action-btn` | Three-dot menu trigger |
| `.row-action-menu` / `.open` | Row action dropdown |
| `.row-action-menu-item` / `.danger` | Action menu items |
| `.row-action-menu--inline` | Absolute-positioned variant |

**Row states:** `tr.selected` (blue-100 bg), `tr:hover` (surface-hover)

### 4.7 Checkbox
| Class | Use |
|---|---|
| `.cb` | Base checkbox 16x16 |
| `.cb.checked` | Checked state |
| `.cb.semi` | Indeterminate/partial |
| `.cb.disabled` | Disabled |

SVG children: `.cb-check` (checkmark), `.cb-semi` (minus)

### 4.8 Search & Filter
| Class | Use |
|---|---|
| `.audit-search` | Search input bar (240px) |
| `.audit-filter-btn` | Filter toggle button |
| `.audit-filter-btn.active` | Active filter state |
| `.audit-filter-btn.skeleton` | Skeleton loading |
| `.filter-badge` | Active filter count pill |

### 4.9 Pagination
| Class | Use |
|---|---|
| `.audit-pagination` | Pagination footer bar |
| `.audit-pagination-left` | Items-per-page area |
| `.audit-pagination-right` | Page navigation area |
| `.audit-page-btn` | Prev/next buttons |
| `.audit-page-select` | Page number select |
| `.no-footer` | Hides pagination (≤10 items) |

### 4.10 Tags, Badges, Chips

**Tags (status — filled bg, no border):**
`.tag`, `.tag-neutral`, `.tag-inactive`, `.tag-secure`, `.tag-success`, `.tag-accent`, `.tag-guide`, `.tag-alert`, `.tag-warn`, `.tag-caution`, `.tag-danger`, `.tag-info`, `.tag-warning`, `.tag-running`, `.tag-cancelled`

**Tags (keyword — bordered, no fill):**
`.tag-keyword`, `.tag-keyword-inactive`, `.tag-keyword-secure`, `.tag-keyword-success`, `.tag-keyword-accent`, `.tag-keyword-guide`, `.tag-keyword-alert`, `.tag-keyword-warn`, `.tag-keyword-caution`

**Tag group:** `.tag-group`

**Badge dots (8px circles):**
`.badge-dot`, `.badge-dot-neutral`, `.badge-dot-inactive`, `.badge-dot-secure`, `.badge-dot-success`, `.badge-dot-accent`, `.badge-dot-guide`, `.badge-dot-alert`, `.badge-dot-warn`, `.badge-dot-caution`, `.badge-dot-skeleton`

**Badge numbers (16px tall):**
`.badge-number`, `.badge-number-neutral` through `.badge-number-caution`, `.badge-number-skeleton`

**Chips (24px height, 2px radius):**
`.chip`, `.chip-default`, `.chip-disabled`, `.chip-inactive`, `.chip-accent`, `.chip-success`, `.chip-secure`, `.chip-guide`, `.chip-alert`, `.chip-warn`, `.chip-caution`, `.chip-skeleton`
`.chip-interior`, `.chip-icon`, `.chip-avatar`, `.chip-avatar-{semantic}`, `.chip-flag`
`.chip-selected`, `.chip-remove`

**Level badges:** `.level-badge` + `.warning`, `.error`, `.info`, `.success`
**Category badge:** `.category-badge`

### 4.11 Scan Status
`.scan-status`, `.scan-status-allowed`, `.scan-status-blocked`, `.scan-status-complete`, `.scan-status-failed`, `.scan-status-skipped`, `.scan-status-pending`

### 4.12 Verdicts
`.verdict`, `.verdict-dot`, `.verdict-text`, `.verdict-flag`

Semantic variants: `.verdict-neutral`, `.verdict-not-active`, `.verdict-secure`, `.verdict-success`, `.verdict-accent`, `.verdict-guide`, `.verdict-alert`, `.verdict-warn`, `.verdict-caution`

Scan-specific: `.verdict-not-detected`, `.verdict-not-supported`, `.verdict-excluded`, `.verdict-detected`, `.verdict-detected-metascan`, `.verdict-progress`, `.verdict-completed`, `.verdict-failed`

### 4.13 Severities
`.severity`, `.severity-icon`
`.severity-critical`, `.severity-high`, `.severity-medium`, `.severity-low`, `.severity-none`

### 4.14 Overlays — Modal
| Class | Use |
|---|---|
| `.modal-overlay` / `.open` | Backdrop |
| `.modal` | Dialog container (480px) |
| `.modal-header` | Header with title + close |
| `.modal-header h2` | Title |
| `.modal-close` | Close button |
| `.modal-divider` | Horizontal rule |
| `.modal-body` | Content area |
| `.modal-field` | Field group |
| `.modal-field-label` | Field label |
| `.modal-footer` | Action button row |
| `.modal-instructions` | Instructional text |

### 4.15 Overlays — Slide Panel
| Class | Use |
|---|---|
| `.slide-panel-overlay` / `.open` | Backdrop |
| `.slide-panel` / `.open` | Panel (400px, right) |
| `.slide-panel-header` | Header bar |
| `.slide-panel-body` | Scrollable content |
| `.slide-panel-footer` | Action bar |
| `.panel-field` | Field group |
| `.panel-field-label` | Label |
| `.panel-banner` | Contextual alert |
| `.panel-banner.warning` / `.error` / `.info` | Variants |

### 4.16 Advanced Filters Panel
| Class | Use |
|---|---|
| `.advanced-filters-overlay` / `.open` | Backdrop |
| `.advanced-filters-panel` / `.open` | Panel (420px) |
| `.af-header` / `.af-close` | Header |
| `.af-body` | Content |
| `.af-section` / `.af-section-title` | Grouped sections |
| `.af-field` / `.af-field-label` | 2-col grid field |
| `.af-chips` / `.af-chip` / `.af-chip.selected` | Filter chip group |
| `.af-footer` / `.af-footer-actions` | Footer bar |

### 4.17 Dropdowns
**Avatar dropdown:** `.avatar-dropdown-wrap`, `.avatar-dropdown` / `.open`, `.avd-user`, `.avd-user-info`, `.avd-user-name`, `.avd-user-email`, `.avd-divider`, `.avd-group`, `.avd-item`, `.avd-submenu` / `.open`, `.avd-back`, `.avd-item.active .avd-check`

**Notification dropdown:** `.notif-dropdown-wrap`, `.notif-dropdown` / `.open`, `.notif-header`, `.notif-header-title`, `.notif-header-action`, `.notif-list`, `.notif-msg`, `.notif-msg-title`, `.notif-msg-body`, `.notif-msg-time`, `.notif-msg-dot`, `.notif-divider`, `.notif-empty`

### 4.18 Toasts
**Bottom toast (snackbar):** `.toast`, `.toast.show`
**Toaster alert (top-right card):** `.toaster`, `.toaster.visible`, `.toaster-info`, `.toaster-success`, `.toaster-error`, `.toaster-content`, `.toaster-text`, `.toaster-title`, `.toaster-desc`, `.toaster-close`

### 4.19 Banners
`.banner` + state: `.banner-info`, `.banner-alert`, `.banner-neutral`, `.banner-warn`
`.banner-icon`, `.banner-content`, `.banner-text`, `.banner-title`, `.banner-desc`
`.banner-actions`, `.banner-actions-divider`, `.banner-link`

### 4.20 Miscellaneous Components
| Class | Use |
|---|---|
| `.status-dot` + `.green` / `.red` | Inline status indicator |
| `.cell-action` | Table action icon |
| `.copy-btn` | Copy-to-clipboard button |
| `.av-badge` | AV engine badge |
| `.tooltip-popover` | Hover tooltip (260px max) |
| `.header-icon` | Header toolbar icon button |
| `.notif-dot` | Red notification indicator |
| `.header-avatar` | User avatar in header |
| `.count-more` | "+N" overflow badge |
| `.expand-btn` / `.open` | Expand/collapse arrow |
| `.theme-toggle` | Dark mode toggle |
| `.priority-toggle` / `.priority-toggle-btn` / `.active` | Segmented toggle |

### 4.21 Navigation & Layout
| Class | Use |
|---|---|
| `.page-view` / `.active` | Page view switching |
| `.page-view.table-page` | Full-height table page |
| `.page-header` | Page title block |
| `.page-title-row` | Title + actions row |
| `.page-title` / `h1` | Page title (24px) |
| `.page-header-actions` | Title-level actions |
| `.breadcrumb` | Breadcrumb nav |
| `.breadcrumb-item` | Clickable crumb |
| `.breadcrumb-sep` | Chevron separator |
| `.breadcrumb-ellipsis` | Overflow indicator |
| `.section-title` | Section heading (20px) |
| `.section-desc` | Section description |
| `.content` | Main content area |

### 4.22 Cards Grid
`.cards-grid` (5-col grid), `.grid-card`, `.grid-card-icon`, `.grid-card-name`, `.grid-card-type`

### 4.23 Info Bar
`.info-bar`, `.info-bar-section`, `.info-bar-divider`, `.info-bar-icon`, `.info-bar-meta`, `.info-bar-label`, `.info-bar-value`, `.info-bar-value-text`

### 4.24 Charts & Data Viz

**Donut chart:** `.chart-donut-group`, `.chart-donut`, `.chart-donut-sep`, `.chart-donut-svg`, `.chart-donut-track`, `.chart-donut-seg`, `.chart-donut-legend`, `.chart-donut-title`, `.chart-donut-value`, `.chart-donut-num`, `.chart-donut-delta`, `.chart-legend-row`, `.chart-legend-dot`, `.chart-legend-label`, `.chart-legend-val`
Delta: `.delta-pos`, `.delta-neg`

**Stat cards:** `.stat-card-row`, `.stat-card`, `.stat-card-body`, `.stat-card-total`, `.stat-card-label`, `.stat-card-value`, `.stat-card-trend` (+ `-up`, `-down`), `.stat-card-trend-text`, `.stat-card-legend`, `.stat-card-legend-row`, `.stat-card-legend-dot`, `.stat-card-legend-label` (+ `.inactive`), `.stat-card-legend-val` (+ `.inactive`)
Bar chart: `.stat-card-chart`, `.stat-card-chart-col`, `.stat-card-chart-label`, `.stat-card-chart-bar`
Semantic variants: `.stat-card--threats`, `.stat-card--dlp`, `.stat-card--sanitized`

**Bar chart (simple):** `.chart-bar`, `.chart-bar-col`, `.chart-bar-label`, `.chart-bar-fill`, `.chart-bar-x-label`

**Stacked bar chart:** `.chart-stacked-legend`, `.chart-stacked-legend-item`, `.chart-stacked-legend-swatch`, `.chart-stacked-tabs`, `.chart-stacked-tab` / `.active`, `.chart-stacked-wrap`, `.chart-stacked-y-axis`, `.chart-stacked-area`, `.chart-stacked-col`, `.chart-stacked-bar`, `.chart-stacked-seg`, `.chart-stacked-x-label`

**Treemap:** `.chart-treemap`, `.chart-treemap-row`, `.chart-treemap-cell`, `.chart-treemap-name`, `.chart-treemap-count`

**Legend panel:** `.chart-legend-panel`, `.chart-legend-header`, `.chart-legend-title`, `.chart-legend-total`, `.chart-legend-rows`, `.chart-legend-item`, `.chart-legend-icon`, `.chart-legend-info`, `.chart-legend-name`, `.chart-legend-bar-wrap`, `.chart-legend-bar`, `.chart-legend-value`

**Progress ring:** `.progress-ring`, `.progress-ring-svg`, `.progress-ring-track`, `.progress-ring-fill`, `.progress-ring-label`

### 4.25 Provider Icons
`.inv-provider`, `.inv-provider-icon` + `.aws`, `.box`, `.alibaba`, `.sharepoint`, `.azure`, `.gcp`, `.onedrive`, `.oracle`, `.netapp`, `.ftp`, `.nfs`, `.smb`, `.sftp`, `.s3c`, `.cubbit`, `.dell`, `.mft`

### 4.26 Column Visibility Panel
`.col-vis-item`, `.col-vis-drag`, `.col-vis-label`, `.col-vis-section-title`

### 4.27 Skeletons
`.skeleton`, `.skeleton-text`, `.skeleton-block`, `.skeleton-btn`
Component-specific: `.btn-text.skeleton`, `.btn-menu.skeleton`, `.radio-option.skeleton`, `.toggle-switch.skeleton`, `.badge-dot-skeleton`, `.badge-number-skeleton`, `.chip-skeleton`, `.audit-filter-btn.skeleton`

### 4.28 WCAG 2.2 AA Components
`.skip-link` — Skip-to-content link
`.sr-only` — Screen-reader only utility
Global `:focus-visible` — 2px solid primary, 2px offset
`@media (prefers-reduced-motion: reduce)` — Disables all animations
`@media (forced-colors: active)` — Windows High Contrast support

---

## 5. ICON INVENTORY (icons.css)

### Usage Pattern
```html
<span class="icon icon-{name}"></span>
```
Icons use CSS `mask-image` and inherit color from `currentColor`. Size with `.icon-sm` (12), `.icon-md` (16), `.icon-lg` (20), `.icon-xl` (24).

### 5.1 Navigation & Directional (maskable)
| Class | Description |
|---|---|
| `.icon-chevron-down` | Dropdown arrow |
| `.icon-chevron-up` | Collapse arrow |
| `.icon-chevron-left` | Back / previous |
| `.icon-chevron-right` | Forward / next / breadcrumb |

### 5.2 Data & Table (maskable)
| Class | Description |
|---|---|
| `.icon-filter` | 3-line filter indicator |
| `.icon-search` | Magnifying glass |

### 5.3 Actions (maskable)
| Class | Description |
|---|---|
| `.icon-action` | Three horizontal dots (more menu) |
| `.icon-add` | Plus sign |
| `.icon-check` | Checkmark |
| `.icon-minus` | Minus / indeterminate |
| `.icon-edit` | Pencil |
| `.icon-close` | X / close |
| `.icon-drag-v` | 6-dot vertical grip |

### 5.4 Controls (maskable)
| Class | Description |
|---|---|
| `.icon-control-h` | Horizontal slider control |
| `.icon-control-v` | Vertical slider control |

### 5.5 Signal Level Icons (multi-color, NOT maskable)
These use `background-image` not `mask-image`. Color is baked in.
| Class | Level | Color |
|---|---|---|
| `.icon-signal-0` | None / no risk | Gray (neutral) |
| `.icon-signal-1` | Low / benign | Dark gray |
| `.icon-signal-2` | Medium / suspicious | Yellow |
| `.icon-signal-3` | High / malicious | Orange |
| `.icon-signal-4` | Critical / malicious | Red |

### 5.6 Feedback Icons (multi-color, NOT maskable)
| Class | Description | Color |
|---|---|---|
| `.icon-fb-info` | Info square with "i" | Navy blue |
| `.icon-fb-success` | Square with checkmark | Blue |
| `.icon-fb-error` | Square with X | Red |

---

## 6. KEY CONVENTIONS

### Two Table Types
- **Small tables** (`.data-table`): Auto layout, ≤5 columns, no horizontal scroll. Columns size naturally.
- **Big tables** (`.data-table.table-fixed`): Fixed layout, 6+ columns, horizontal scroll via `.table-scroll` wrapper. Supports pinned columns (`.col-pinned`, `.col-action`). Set `min-width` on the `<table>` to prevent column crushing.

### Table Column Pinning
- `.col-pinned` — sticky left (checkbox at `left: 0`, name at `left: 48px`)
- `.col-action` — sticky right (56px, action menu ...)
- Pinned columns need opaque backgrounds (`--surface-card`) so content doesn't show through

### Card Headers (Two Tiers)
- `.card-header` — simple cards (settings panels, job sections). 56px height, title + optional actions.
- `.audit-card-header` — data-table cards with bulk actions, column visibility, search, filters.

### Tag Variants (9 Semantic Colors)
`.tag-neutral`, `.tag-inactive`, `.tag-secure`, `.tag-success`, `.tag-accent`, `.tag-guide`, `.tag-alert`, `.tag-warn`, `.tag-caution` — all auto-adapt to dark mode via `--ds-*` tokens.

### Dark Mode
- Activated by `data-theme="dark"` on `<html>`
- All semantic tokens override in tokens.css
- Component-level dark overrides are colocated in components.css (not a separate file)
- Always test both themes when adding/changing components

### Buttons
- All buttons are **32px height** — use explicit `height: 32px` with `padding: 0 Xpx` and flexbox centering. Never use vertical padding to derive height.
- Gap between buttons: **8px**
- Internal icon-text gap: **8px**
- Naming: `.btn-primary`, `.btn-outline`, `.btn-text`, `.btn-menu`, `.btn-icon`, `.btn-brand`, `.btn-danger`

### Page Title Row Rules
- If the `.page-title-row` has **3 buttons** on the right and one is Refresh, make Refresh an **icon-only button** (`.btn-icon`)

### Padding & Spacing
- Card body padding: `var(--space-std)` (20px)
- Table first/last column outer padding: 20px
- Form row gaps: `var(--space-sm)` (12px)
- Standard gutter: `var(--space-gutter)` (12px)

### Typography
- Body font: `var(--font)` → Roboto
- Brand font: `var(--font-brand)` → Simplon Norm (sidebar footer, branding only)
- Buttons, labels, inputs: 14px, weight 500
- Body text: 14px, weight 400
- **Font sizes must be rounded** — no decimals. Figma outputs like `15.96px` must be rounded to `16px`.

### Token Pitfalls
- `--text-link` equals `--blue-800` (#1854c3) — don't use `--blue-800` for hover states, use `--blue-900` (#123d8b)
- Dark mode hover for `.btn-text`: use `--blue-400`
- Dark mode hover for `.btn-menu`: border `--neutral-400`

---

## 7. PROTOTYPING STANDARDS

### 7.1 HTML Scaffold
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="https://vilevich.github.io/blue-line/tokens.css">
  <link rel="stylesheet" href="https://vilevich.github.io/blue-line/components.css">
  <link rel="stylesheet" href="https://vilevich.github.io/blue-line/icons.css">
  <title>[Prototype Name]</title>
</head>
<body>
  <a href="#main" class="skip-link">Skip to content</a>
  <!-- header, sidebar, content -->
  <main id="main" class="content">
    <!-- prototype content -->
  </main>
</body>
</html>
```

### 7.2 Dark Mode Toggle Pattern
```javascript
document.querySelector('.theme-toggle').addEventListener('click', () => {
  const html = document.documentElement;
  html.getAttribute('data-theme') === 'dark'
    ? html.removeAttribute('data-theme')
    : html.setAttribute('data-theme', 'dark');
});
```

### 7.3 Interaction Pattern Rules
- All interactive elements must have `:focus-visible` styles (already built into components.css)
- Minimum touch target: 24x24px (WCAG 2.5.8 — enforced in components.css)
- Transitions respect `prefers-reduced-motion` (built in)
- Modals trap focus; slide panels use overlay dismiss
- Tables: checkbox column sticky left, action column sticky right
- Pagination hides when ≤10 items (`.no-footer` on `.audit-card`)

### 7.4 Gap Flagging Protocol
When a prototype needs something not in the DS files:
1. Stop. Do not improvise.
2. Flag: "**DS GAP:** [description of what's needed]"
3. Suggest: which existing component/token could be extended
4. Log in Decision Log (Section 11) for design system backlog

---

## 8. ACCESSIBILITY REQUIREMENTS

### 8.1 WCAG 2.2 AA Compliance (Built into Blue Line)
| Criterion | Implementation |
|---|---|
| 1.4.3 Contrast minimum | `--text-muted-accessible` (#566479) passes 4.5:1 on white |
| 1.4.12 Text spacing | Toaster desc uses `line-height: 1.5` |
| 2.4.7 Focus visible | Global `:focus-visible` with `--primary` ring |
| 2.4.11 Focus not obscured | `outline-offset: 2px` on all interactive elements |
| 2.5.8 Target size | Min 24x24px enforced on toggles, radios, icons |
| 3.3.1 Error identification | `.input-field.error` + `.input-error-msg` |

### 8.2 Prototype Requirements
- Every interactive element must be keyboard-operable
- Use `<button>` for actions, `<a>` for navigation — never `<div>` with `onclick`
- Modals: `role="dialog"`, `aria-modal="true"`, focus trap
- Tables: `<th scope="col">`, `aria-sort` on sortable columns
- Status changes: `aria-live="polite"` for toast/toaster regions
- Form validation: `aria-invalid="true"`, `aria-describedby` linking to error messages
- Disabled: `.disabled` class + `aria-disabled="true"` (not just `:disabled`)
- SR-only text: `.sr-only` class for icon-only buttons

---

## 9. CROSS-PRODUCT CONSISTENCY NOTES

### 9.1 Table Patterns
- All OPSWAT products use the same table anatomy: `.audit-card` > `.audit-card-header` > `.table-scroll` > `.data-table` > `.audit-pagination`
- Checkbox + pinned name + action columns are the standard for big tables
- Column filter icons always position absolute right inside `<th>`
- Row actions: three-dot `.row-action-btn` in `.col-action`

### 9.2 Status Representation Hierarchy
1. **Scan status** (`.scan-status-*`) — for workflow/process states
2. **Tags** (`.tag-*`) — for entity states in tables
3. **Verdicts** (`.verdict-*`) — for inline dot+text results
4. **Badges** (`.badge-dot-*`, `.badge-number-*`) — for counts and indicators
5. **Severities** (`.severity-*` + `.icon-signal-*`) — for risk levels

### 9.3 Card Header Variants
| Variant | When to use |
|---|---|
| `.card-header` | Simple settings cards |
| `.audit-card-header` | Data-table cards |
| `.audit-card-header.with-desc` | Cards needing subtitle |
| `.audit-card-header.selected` | Bulk action active |

---

## 10. CONTRIBUTING

### Adding New Components
1. Add styles to `components.css` following existing patterns
2. Include dark mode override colocated with the component (use `[data-theme="dark"]` selector)
3. Use existing token variables — never hardcode colors or spacing
4. Include `:focus-visible` states for all interactive elements
5. Ensure touch targets are at least 24x24px (WCAG 2.5.8)
6. Add an example to `index.html` docs page
7. Create or update `docs/components/[name].md`
8. Update `CHANGELOG.md`
9. Test in both light and dark mode

### Component Maturity
- **Stable** — used in production, API locked, changes are backward-compatible
- **Beta** — used in at least one product, API mostly stable
- **Alpha** — new, API may change significantly

### Branch Workflow
- `main` = stable, deployed to GitHub Pages
- `feature/component-name` = work in progress
- PRs require reviewer approval before merge to main

### Versioning
Follows [Semantic Versioning](https://semver.org/):
- **Major** = breaking changes (rare)
- **Minor** = new components, new tokens
- **Patch** = bug fixes, doc updates, WCAG improvements

---

## 11. RESPONSIVE BREAKPOINTS (Active)

| Breakpoint | Max-width | Behavior |
|---|---|---|
| Medium | 1460px | Reduce content padding |
| Small/Tablet | 1024px | Stack form rows, scrollable tabs |
| Mobile | 768px | Full-width panels, wrapped radios |
| Small Mobile | 480px | Compact card padding |
| Chart 1200px | 1200px | 2-col stat cards |
| Chart 900px | 900px | Stack donuts, info bars, 1-col stats |
| Chart 600px | 600px | Shorter stacked charts, hide tabs |

---

## 12. DECISION LOG

| # | Date | Decision | Rationale | Status |
|---|---|---|---|---|
| 1 | 2026-03-09 | Full token inventory in claude.md | Eliminates file-read latency; self-contained reference | Active |

---

## 13. UX PATTERN DECISIONS & RATIONALE

_Populated as prototypes are built and patterns validated._

| Pattern | Decision | Rationale | Prototype |
|---|---|---|---|
| — | — | — | — |

---

## 14. OPEN ITEMS & THREADS

| # | Item | Status | Owner | Notes |
|---|---|---|---|---|
| — | — | — | — | — |

---

## 15. DS GAP BACKLOG

_Components or tokens needed but not yet in Blue Line._

| # | Gap | Workaround | Priority | Filed |
|---|---|---|---|---|
| — | — | — | — | — |

---

_Last updated: 2026-03-09_
_Blue Line version: 2.2.2_
