# IronShield Army — Design System v2.0
> Released: 2026-04-16 · Updated: 2026-04-19 (font switch) · Replaces v1.0 (April 2026) · Owner: brand
> Live implementation: `assets/ironshield-ds-tokens.css` on theme `SebekMiniBuilder`
> Figma source of truth: (file URL added after creation)

---

## What changed vs v1

| Area | v1 | v2 |
|---|---|---|
| Typography | Inter only, all 400 weight | **Inter only**, weights 400/500/600/700 (Inter Bold 700 = display) |
| Tokens | Inline in docs | **`assets/ironshield-ds-tokens.css`** — actual CSS custom properties, loaded globally |
| Surfaces | White + F5F5F5 | **+ Parchment (#F7F0E4), Mystic indigo (#172351), Forge (#0F0E0D)** |
| Buttons | Primary/secondary, no visual depth | **+ Forge variant** (gradient, inset shadow) for hero CTAs; proper focus ring |
| Motion | Not specified | **Tokens: fast/base/slow + custom easing curves** |
| Shadows | None | **5-step system + copper glow** |
| Radii | 0 / 40 | **0, 2, 4, 8, 14, 999 (pill)** — gives finer hierarchy |
| Focus | Browser default | **Dual-ring (copper core + soft halo)** — WCAG 2.1 AA |
| New components | — | Trust row, sticky ATC + free-shipping progress, ornamental divider, price with savings badge, section kicker+heading |

---

## 1. Why the redesign

Pain points from Apr 2026 intelligence report:
- **Checkout > Purchase dropped to 40% in April** (was 55% in March)
- **AOV = €64.88** — shipping threshold is €49, so many carts flirt with free-shipping line and abandon
- **ROAS declining (-25% since Feb)** — traffic is still there, conversion is slipping
- Homepage + PDP have very low visual hierarchy (all text 400 weight, no accent usage, minimal shadow)

Design response:
1. **Stronger hierarchy** via Inter Bold 700 (display) + variable weights — scannable PDP / hero
2. **Sticky ATC with free-shipping progress bar** — directly attacks cart-shock abandonment
3. **Trust row** surfacing "169g avg, 5–7 day ship, 4,005+ customers" (data from report)
4. **Forge button variant** for hero CTA — raises perceived quality of the "miniature" product

---

## 2. Design tokens

Full list implemented at `/cdn/.../assets/ironshield-ds-tokens.css`. Key tokens:

### Color

```css
--ish-accent:    #C6682D   /* copper — primary CTA, sale, links */
--ish-accent-600:#A7551F   /* hover state */
--ish-blue:      #334FB4   /* secondary */
--ish-blue-dark: #172351   /* mystic indigo bg */

--ish-fg:        #121212
--ish-bg:        #FFFFFF
--ish-bg-alt:    #F5F5F5
--ish-bg-parchment: #F7F0E4   /* new — warm paper bg for DM/rules cards */
--ish-bg-mystic:    #172351   /* new — deep indigo hero panels */
--ish-bg-forge:     #0F0E0D   /* new — near-black dramatic sections */

--ish-sale:      #C6682D
--ish-star:      #FFAD00
--ish-success:   #27AE60
--ish-error:     #C0392B
--ish-warning:   #E67E22
```

### Typography

```css
--ish-font-body:    'Inter', system-ui, sans-serif;
--ish-font-display: 'Inter', ui-sans-serif, system-ui, sans-serif;   /* same family as body, Bold 700 weight */

/* Fluid sizes */
--ish-text-display: clamp(2rem,   1.3rem + 2.8vw, 3.5rem);  /* H1 hero */
--ish-text-heading: clamp(1.5rem, 1.1rem + 1.8vw, 2.25rem); /* H2 */
--ish-text-sub:     clamp(1rem,   0.9rem + 0.5vw, 1.15rem); /* lead paragraph */

/* Fixed steps */
--ish-text-xs:   12px
--ish-text-sm:   13px
--ish-text-base: 16px
--ish-text-lg:   18px
--ish-text-xl:   20px
--ish-text-2xl:  24px
--ish-text-3xl:  32px
--ish-text-4xl:  40px
--ish-text-5xl:  52px  /* hero */
```

**Pairing rules:**
- Hero H1 / Section H1 → **Inter Bold 700**, letter-spacing `-0.01em`
- Section H2 → **Inter 600**
- Body → **Inter 400** line-height 1.5
- Kicker (above heading) → **Inter 600**, `0.18em` tracking, uppercase, `13px`
- Price → **Inter 600** 18px for value, 13px strikethrough compare

### Spacing (4px base)

```
1=4  2=8  3=12  4=16  5=20  6=24  8=32  10=40  12=48  16=64  20=80
```

### Radii

```
0, 2 (sm), 4 (md), 8 (lg), 14 (xl), 999 (pill — chips, variant pills)
```

### Shadows

```css
--ish-shadow-xs:    0 1px 2px rgba(0,0,0,0.05)
--ish-shadow-sm:    0 2px 8px rgba(15,14,13,0.06)
--ish-shadow-md:    0 6px 20px rgba(15,14,13,0.10)    /* sticky ATC */
--ish-shadow-lg:    0 18px 40px rgba(15,14,13,0.14)
--ish-shadow-glow:  0 0 0 3px var(--ish-accent-300)   /* hover/focus */
--ish-shadow-inset: inset 0 1px 0 rgba(255,255,255,0.06)
--ish-focus-ring:   0 0 0 3px var(--ish-accent-300), 0 0 0 5px var(--ish-accent)
```

### Motion

```css
--ish-motion-fast:   120ms   /* button press */
--ish-motion-base:   180ms   /* hover, color change */
--ish-motion-slow:   320ms   /* slide-in, drawer */
--ish-ease-standard: cubic-bezier(0.2, 0.0, 0.0, 1)
--ish-ease-emphasis: cubic-bezier(0.3, 0.0, 0.0, 1.15)  /* small overshoot */
```

### Z-index

```
base=1 · header=50 · dropdown=100 · sticky-atc=200 · overlay=900 · modal=1000 · toast=1100
```

---

## 3. Component library

All components are prefixed `.ish-` to avoid collision with existing theme styles. Drop classes directly into Liquid templates.

### 3.1 Button

Base: `.ish-btn`
Variants: `.ish-btn--primary` (copper), `.ish-btn--ghost` (outline), `.ish-btn--forge` (gradient — hero CTA)
Sizes: `.ish-btn--sm`, `.ish-btn--lg`
Modifiers: `.ish-btn--block` (full-width)

```html
<button class="ish-btn ish-btn--forge ish-btn--lg ish-btn--block">
  Forge your army — from €7
</button>
```

All buttons have copper focus ring (`--ish-focus-ring`).

### 3.2 Chip (also filter chip)

`.ish-chip` + optional `.ish-chip--active`
Used in: collection banner filter chips, category browse, autocomplete tokens.

### 3.3 Badge

`.ish-badge` + variant: `--sale` (copper), `--new` (blue), `--rare` (indigo), `--outline` (transparent).
22px tall, uppercase, `0.12em` tracking. Use sparingly (max 1 per card).

### 3.4 Price

```html
<div class="ish-price">
  <span class="ish-price__value">€12.90</span>
  <span class="ish-price__compare">€18.00</span>
  <span class="ish-price__savings">−28%</span>
</div>
```

### 3.5 Trust row (NEW)

5-item icon row for below-header placement. Surfaces:
- Hand-crafted in Poland 🇵🇱
- 12K / 14K resin detail
- Free EU shipping €49+
- 4,005+ DMs & players
- Ships in 5–7 days

Auto-fits 2–5 columns based on viewport via `grid-template-columns: repeat(auto-fit, minmax(180px, 1fr))`.

### 3.6 Sticky ATC + free-shipping progress (NEW — conversion fix)

Fires on product pages when user scrolls past the main ATC button. Shows:
- Free-shipping progress bar (fills as you add qty)
- Label: "You're €X away from free shipping"
- Mini-ATC button that scrolls back to selector / adds current variant

Directly addresses the 40% checkout drop mentioned in the April report.

### 3.7 Section kicker + heading

```html
<p class="ish-section-kicker">Forged for Legends</p>
<h2 class="ish-section-heading">New arrivals this week</h2>
```

Kicker in copper, heading in Inter Bold 700 — gives every section a clear start.

### 3.8 Collection hero (already live)

Banner image + heading + subtext + clickable class filter chips. See `sections/collection-hero-ironshield.liquid`.

### 3.9 Card

`.ish-card` with image inside `.ish-card__media` (aspect 1/1, border hair-thin, hover = scale 1.04 + copper-tint bg).

### 3.10 Input

`.ish-input` — 44px tall, dark outline, copper focus ring.

### 3.11 Ornamental divider

```html
<div class="ish-divider">VII · Volumes of the Codex</div>
```

Horizontal line left, chapter number center, line right — use between long-form PDP sections.

---

## 4. Recommended upgrade paths on the live store

| Priority | Screen | Change | Component to use |
|---|---|---|---|
| 🔴 High | **PDP** | Add sticky ATC + free-shipping progress | `.ish-sticky-atc` |
| 🔴 High | **PDP** | Apply forge variant to "Add to Cart" | `.ish-btn--forge` |
| 🟠 Mid | **Homepage below header** | Trust row | `.ish-trust-row` |
| 🟠 Mid | **Homepage hero** | Inter Bold H1, copper kicker | `.ish-section-kicker` + `.ish-section-heading` |
| 🟠 Mid | **Product card** | Replace price block with `.ish-price` | `.ish-price*` |
| 🟢 Nice | **Collection card** | Use `.ish-card` hover scale | `.ish-card` |
| 🟢 Nice | **Footer** | Dark-mode (forge bg) alt | manual |
| 🟢 Nice | **Search results** | `.ish-chip` for recent searches | `.ish-chip` |

---

## 5. Accessibility checklist

- [x] **Contrast**: #C6682D on #FFFFFF = 4.57:1 (AA pass for large text & UI) · #121212 on #FFFFFF = 17.1:1 (AAA)
- [x] **Focus ring**: dual-ring copper ensures visibility on both light and dark surfaces
- [x] **Touch targets**: minimum 44px (btn, chip, input) — passes WCAG 2.5.5
- [x] **Reduced motion**: `@media (prefers-reduced-motion: reduce)` disables transitions/animations
- [x] **Font loading**: `font-display: swap` via Inter Google Fonts import → no FOIT

---

## 6. File map

```
Shopify theme "SebekMiniBuilder":
  assets/
    ironshield-ds-tokens.css       ← tokens + primitives (368 lines)
  layout/
    theme.liquid                    ← injects Inter font + DS CSS
  sections/
    collection-hero-ironshield.liquid ← uses --ish-* tokens
```

Local repo:
```
IRONSHIELD_DESIGN_SYSTEM.md        ← v1, kept for history
IRONSHIELD_DESIGN_SYSTEM_v2.md     ← THIS FILE (replace reference)
IronShield_Army_Report.md          ← intel that shaped v2
```

Figma:
```
IronShield Army — Design System (file key added after creation)
├── Cover
├── Tokens (colors, type scale, spacing, radii)
├── Components (buttons, chips, cards, badges, inputs, price, banner)
└── Sample screens (Homepage hero, PDP, Collection page)
```

---

## 7. Principles

1. **Function > decoration** — fantasy aesthetic comes from the product imagery, not the chrome
2. **Inter carries everything** — Bold 700 weight reserved for H1/brand moments
3. **Copper, not gold** — `#C6682D` (not yellow) — keeps the brand feeling forged, not gilded
4. **Motion in moderation** — nothing over 320ms, respects reduced-motion preference
5. **Shopify-compatible** — all styles use regular CSS (no build step), works in theme editor, no JS dependencies
