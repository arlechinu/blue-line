# Verdicts

**Status:** Beta

Inline verdict indicators showing a colored dot or flag icon paired with a text label. Used for scan results, detection statuses, and origin verdicts.

---

## Anatomy

```
┌────────────────────────────┐
│ [●/🏴]  Verdict Text       │
└────────────────────────────┘
```

- **Indicator** — 6×6 dot (`.verdict-dot`) or flag icon (`.verdict-flag`)
- **Text** — 14px, either `--text-strong` (active) or `--neutral-500` (muted)

## Usage

```html
<!-- Dot variant -->
<span class="verdict verdict-secure">
  <span class="verdict-dot"></span>
  <span class="verdict-text">Secure</span>
</span>

<!-- Flag variant -->
<span class="verdict verdict-flag-neutral">
  <span class="verdict-flag fi fi-us"></span>
  <span class="verdict-text">United States</span>
</span>
```

## Generic Variants (9 Semantic Colors)

| Class | Dot color (light) | Dot color (dark) | Text |
|-------|-------------------|-------------------|------|
| `.verdict-neutral` | `--neutral-700` | `--neutral-700` | muted |
| `.verdict-not-active` | `--neutral-200` | `--neutral-900` | muted |
| `.verdict-secure` | `--blue-700` | `--blue-400` | strong |
| `.verdict-success` | `--green-700` | `--green-400` | strong |
| `.verdict-accent` | `--teal-700` | `--teal-400` | strong |
| `.verdict-guide` | `--purple-700` | `--purple-400` | strong |
| `.verdict-alert` | `--red-700` | `--red-400` | strong |
| `.verdict-warn` | `--orange-700` | `--orange-400` | strong |
| `.verdict-caution` | `--yellow-700` | `--yellow-400` | strong |

## Flag Variants

| Class | Text color |
|-------|-----------|
| `.verdict-flag-neutral` | muted (`--neutral-500`) |
| (no extra class) | strong (`--text-strong`) |

Use `.verdict-flag` with a flag-icons class (e.g., `fi fi-us`) for origin/country verdicts.

## Scan-Specific Variants

| Class | Dot | Text |
|-------|-----|------|
| `.verdict-not-detected` | `--neutral-700` | muted |
| `.verdict-not-supported` | `--neutral-700` | muted |
| `.verdict-excluded` | `--neutral-700` | muted |
| `.verdict-detected` | `--red-700` | strong |
| `.verdict-detected-metascan` | `--red-700` | strong |
| `.verdict-progress` | `--blue-700` | strong |
| `.verdict-completed` | `--green-700` | strong |
| `.verdict-failed` | `--red-700` | strong |
| `.verdict-origin-blocked` | flag | strong |
| `.verdict-origin-allowed` | flag | muted |

## Dark Mode

- Active dots lighten from `-700` to `-400` palette level
- `.verdict-not-active` dims to `--neutral-900` dot / `--neutral-800` text
- Text using `--text-strong` auto-adapts (dark → white)
- Muted text (`--neutral-500`) stays the same in both themes

## Accessibility

- Color is not the sole indicator — text label always accompanies the dot
- Dot meets 3:1 contrast against surface in both themes
- Flag icons provide country context beyond color
