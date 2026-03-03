# Severities

**Status:** Beta

Severity indicators that pair a multi-colored signal icon with a text label. Used across OPSWAT products for threat risk, risk level, and general severity displays.

---

## Anatomy

```
┌─────────────────────────────┐
│ [signal-icon]  Label Text   │
└─────────────────────────────┘
```

- **Signal icon** — one of 5 fixed-color `icon-signal-*` classes (16×16)
- **Label** — 14px regular weight, `--text-subtle` color

## Usage

```html
<span class="severity">
  <span class="icon-signal-4"></span>
  Critical
</span>
```

The `.severity` class provides the inline-flex layout (8px gap, 14px font, `--text-subtle` color). The signal icon class handles its own size and color.

## Signal Icon Levels

| Class | Color | Hex | Use cases |
|-------|-------|-----|-----------|
| `icon-signal-4` | Red | #FF003D | Critical, Malicious, High Risk |
| `icon-signal-3` | Orange | #FF6B00 | High, Likely Malicious, Medium Risk |
| `icon-signal-2` | Yellow | #DBB600 | Medium, Suspicious, Low Risk |
| `icon-signal-1` | Gray | #607497 | Low, Benign, No Risk |
| `icon-signal-0` | Light gray | #A9B2C4 | None, No Threat, No Risk Score |

## Variants

### Severity Levels

Standard 5-level severity scale:

```html
<span class="severity"><span class="icon-signal-4"></span>Critical</span>
<span class="severity"><span class="icon-signal-3"></span>High</span>
<span class="severity"><span class="icon-signal-2"></span>Medium</span>
<span class="severity"><span class="icon-signal-1"></span>Low</span>
<span class="severity"><span class="icon-signal-0"></span>None</span>
```

### Scan Severities — Threat Risk

```html
<span class="severity"><span class="icon-signal-4"></span>Malicious</span>
<span class="severity"><span class="icon-signal-3"></span>Likely Malicious</span>
<span class="severity"><span class="icon-signal-2"></span>Suspicious</span>
<span class="severity"><span class="icon-signal-1"></span>Benign</span>
<span class="severity"><span class="icon-signal-0"></span>No Threat</span>
```

### Scan Severities — Risk Level

```html
<span class="severity"><span class="icon-signal-4"></span>High Risk</span>
<span class="severity"><span class="icon-signal-3"></span>Medium Risk</span>
<span class="severity"><span class="icon-signal-2"></span>Low Risk</span>
<span class="severity"><span class="icon-signal-1"></span>No Risk</span>
<span class="severity"><span class="icon-signal-0"></span>No Risk Score</span>
```

## Dark Mode

Signal icons use fixed SVG colors (not `currentColor`), so they do not change between themes. The label text uses `--text-subtle`, which auto-adapts to dark mode.

## Accessibility

- Signal icons are decorative; the text label conveys meaning
- Color is not the sole indicator — each level has a distinct icon shape (fill amount / slash)
- Touch target meets 24×24 minimum when used in clickable contexts
