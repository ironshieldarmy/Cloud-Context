# IronShield Army — PDP Redesign Proposal
> Date: 2026-04-17 · Owner: IronShield Army · Baseline: current `main-product` template on theme `SebekMiniBuilder`
> Test URL: `/products/wicked-treants-yilvorys-forest-monsters-...` (used for inventory)

---

## 1 · Co już masz na PDP — pełna inwentaryzacja

### Sekcje strony (w kolejności renderowania)

| # | Sekcja | Zawartość | Uwaga |
|---|---|---|---|
| 1 | `ironshield-pdp-hook` | Sticky ATC bar (portal do body) | ✅ DS |
| 2 | Apps #1 | **Dealeasy CVG progress bar** | "spend X more to unlock Y" — obecnie nad tytułem produktu |
| 3 | **Main product** (core) | Vendor + H1 + price + variants + qty + buy + Judge.me stars + share + Subscriptions + Essential estimated delivery + Dealeasy gift-with-product + description | 12 bloków w jednej kolumnie |
| 4 | Apps #2 | **Selleasy** — upsell widget (bought together) | Zaraz pod ATC |
| 5 | `multicolumn_jgDBAf` | **"UNLOCK BONUSES 🎁"** — 4 karty: FREE LOOT CHEST · FREE MYSTERY MINI (>19€) · FREE DELIVERY (>49€ / >99€ USA) · ANOTHER FREE MINI (>69€) | Świetny motywator zakupów |
| 6 | Apps #3 | Widget-builder-all-in-one | ? |
| 7 | Apps #4 | **Judge.me** — pełny review widget | Recenzje |
| 8 | `related-products` | "You may also like" (4 col) | Cross-sell |
| 9–14 | Apps × 6 | GoogleYouTube discounts, inne pixele | Bez UI impact |
| 15–18 | Footer-group (4 sekcje) | Global footer | OK |

### Core product info blocks (wewnątrz sekcji `main`)

Obecnie **jedna kolumna** pod główną treścią produktu:
1. **Vendor** (`DM Stash` – uppercase, 12px)
2. **Title** (H1 + H2.h1 duplikat dla sticky-info)
3. **Price** (`price--large`, z sale badge)
4. **Variant picker** (button style, circle swatch)
5. **Quantity selector**
6. **Buy buttons** (ATC + dynamic checkout)
7. **Judge.me preview badge** (mini gwiazdki)
8. **Share** (native share API)
9. **Subscriptions app** (Spurit)
10. **Essential estimated delivery**
11. **Dealeasy gift-with-product**
12. **Description** (body_html — 679 znaków)

### Po sekcji main (długi scroll)

- **Multirow 6 wierszy** (alternate-left) — osobne sekcje z H2 + obrazem + tekstem:
  - "Top Quality" — 16K resolution, SLA, ABS-like resin
  - "Ready for Painting" — washed, dried, UV-cured
  - "Battle-Ready Shipping" — premium fillers, secure boxes
  - "Scale Your Way" — 28mm / 32mm / 38mm / 75mm
  - "Collector's Value" — base + sticker + bonus item
  - "Worldwide & Reliable" — EU 3-7 days, US 14-30 days
- **Judge.me pełna ściana recenzji**
- **Related products** 4-col
- 6× apps (w większości pixel/tag bez UI)

---

## 2 · Diagnoza — co szwankuje

| Problem | Dane | Dlaczego to boli |
|---|---|---|
| **Zbyt długa pojedyncza kolumna** | 12 bloków w main + 6 wierszy multirow | Użytkownik scrolluje 3–4 ekrany zanim dojdzie do recenzji. Decyzja "buy" rozwleczona. |
| **ATC jest głęboko** | Pod variant/qty, a te pod price, pod title, pod vendor | Na mobilnej klasie > 900px od góry. Sticky ATC (już dodany przeze mnie) częściowo łata. |
| **"View full details" jako literal text** | W body_html | Wygląda jak tekst z importu BaseLinker. Trzeba schować/usunąć. |
| **Dealeasy progress bar NAD tytułem** | Sekcja #2 | Pierwszy element, który widzi klient = upsell bar zamiast produktu. Moved below ATC zarabia więcej. |
| **6 wierszy multirow** | "Top Quality", "Ready for Painting"... | Kluczowe value props (12K/14K resin, 5–7 day ship, scale options) zakopane 4 ekrany niżej. Powinny być obok ceny. |
| **Brak visible spec table** | Tylko proza w opisie | Klient nie wie: waga? wysokość? skala standardowa? base included? → porzucony koszyk. |
| **Judge.me gwiazdki rozproszone** | Preview badge w main + pełny widget daleko | Preview widget shows count tylko jeśli ≥1 review. Gdy produkt bez recenzji → martwy element zabierający miejsce. |
| **Subscriptions + Essential + Dealeasy gift** w main | 3 app blocks pomiędzy buy button a description | "Choice paralysis" — za dużo elementów decyzyjnych na raz. |
| **Multirow = identyczny dla wszystkich produktów** | Te same 6 wierszy na 1480 PDP | Google widzi duplicate content + irytuje returning customer. |

---

## 3 · Trendy PDP 2025/26 — co robią leaderzy

### Z research (Baymard, NN/g, Shopify + własna analiza typowych design patterns)

**Must-have above the fold (desktop 1440×900):**
1. Breadcrumb + category kicker (copper, uppercase)
2. Produkt **title** w display font (Cinzel dla nas)
3. Price z save badge + shipping/tax note
4. Star rating (average + count) — inline z ceną
5. **Variant picker** widoczny bez scrolla
6. **ATC primary CTA** — forge gradient, full-width kolumny
7. **Quick trust row** (3–4 ikony: free EU ship, ships X days, handcrafted)
8. **Gallery** — lewa strona, square, 1:1, z thumbnails

**Below the fold (in order):**
1. Rich specs/tech-details (2-column key-value table)
2. Full description (collapsed "Read more" na mobile)
3. **Scarcity / social proof strip** — "X people bought today", "Only Y left"
4. **Reviews** (Judge.me) — średnia + 3 najnowsze z "Show all →"
5. **"You may also like"** — related 4-col
6. **FAQ accordion** — shipping, printing, returns
7. Cross-sell / upsell widget (bundle savings)

**Anti-patterns (wg NN/g):**
- Inconsistent variant info
- Excessive "fancy features" (virtual try-ons, 360° jeśli nie perfekcyjnie wdrożone)
- Hidden availability na wariantach
- Insufficient product details → return rate up

**Gaming / hobby / collectibles specyfika (IronShield sector):**
- **Scale comparison visual** — 28mm obok 32mm obok 75mm z człowiekiem (ref Games Workshop)
- **Unpainted vs painted comparison** (ref Reaper Miniatures)
- **Class/role tagi** ("Paladin · Fighter · Knight") = filtrable
- **"DM approved" / "Painter's choice" social proof badges**
- **Stat card** (D&D-style) — height, weight, base size, resin type
- **Community photos / hashtag widget** (#IronShieldArmy)

---

## 4 · Propozycja nowego układu

### Desktop (≥ 990px) — **2-kolumnowy sticky layout**

```
┌────────────────────────────────────────────────────────────────┐
│ [ HEADER (nie zmieniamy) ]                                     │
├────────────────────────────────────────────────────────────────┤
│ Home · Figures · Dragons · Wicked Treants    ← breadcrumb      │
├───────────────────────────────┬────────────────────────────────┤
│                               │ PALADIN · FIGHTER  ← kicker cop│
│                               │                                │
│                               │ Wicked Treants ← CINZEL H1     │
│                               │ Yilvorys Forest Monsters       │
│        [BIG IMAGE 1:1]        │                                │
│      (sticky LEFT column)     │ ★★★★★ (127 reviews)            │
│                               │                                │
│                               │ €28.90  €12.90 [−31%]          │
│                               │ Taxes incl · Free EU ship €49+ │
│  ┌──┬──┬──┬──┐                │                                │
│  │T1│T2│T3│T4│ thumbnails     │ SCALE                          │
│  └──┴──┴──┴──┘                │ [28mm][32mm*][75mm][128mm]     │
│                               │                                │
│                               │ QTY [-] 1 [+]                  │
│                               │                                │
│                               │ ┌────────────────────────────┐ │
│                               │ │ FORGE · ADD TO CART €12.90 │ │ ← forge CTA
│                               │ └────────────────────────────┘ │
│                               │ [ Buy it now (Shop Pay) ]      │
│                               │                                │
│                               │ ╔══════════════════════════╗   │
│                               │ ║ ● Ships 5–7 days         ║   │ ← trust row
│                               │ ║ ● 12K/14K resin · POL    ║   │   (parchment bg
│                               │ ║ ● 100-day return         ║   │    only in right col,
│                               │ ║ ● 4,005+ DMs served       ║   │    3–4 dots)
│                               │ ╚══════════════════════════╝   │
│                               │                                │
│                               │ ┌── Product specs ──────────┐  │
│                               │ │ Height       45 mm         │  │ ← tech spec
│                               │ │ Base incl.   32 mm round   │  │   table
│                               │ │ Resin        12K SLA       │  │
│                               │ │ Assembly     separate parts│  │
│                               │ │ Recommended  DnD / Pathfinde│  │
│                               │ └────────────────────────────┘  │
└───────────────────────────────┴────────────────────────────────┘
│                                                                 │
│ ┌───── Description ──────────────────────────────────────────┐ │ ← full-width
│ │ Wicked Treants — Corrupted Guardians of the Bog…          │ │
│ │ [twoja proza 679 znaków — zostaje as-is]                   │ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                 │
│ ┌───── Judge.me reviews ─────────────────────────────────────┐ │
│ │  Average 4.8 ★ (127) · [3 most helpful cards] · View all → │ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                 │
│ ┌───── Unlock Bonuses 🎁 ────────────────────────────────────┐ │
│ │ (istniejąca 4-col multicolumn — zostaje)                   │ │ ← scarcity +
│ │ FREE LOOT CHEST · FREE MINI ≥19€ · FREE DELIVERY ≥49€ · +  │ │   reward
│ └────────────────────────────────────────────────────────────┘ │
│                                                                 │
│ ┌───── Why IronShield ───────────────────────────────────────┐ │
│ │ (multirow 6 wierszy → accordion/tabs 4 nagłówki)           │ │ ← skrócone
│ │ Tabs: Quality · Painting · Shipping · Scale                │ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                 │
│ ┌───── Community gallery (hashtag #IronShieldArmy) ──────────┐ │ ← NEW
│ │ (opcjonalne — scroll 6–8 customer photos)                  │ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                 │
│ ┌───── You may also like ────────────────────────────────────┐ │
│ │ (related-products 4-col — zostaje)                         │ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                 │
│ ┌───── FAQ accordion ────────────────────────────────────────┐ │ ← NEW
│ │ › How long is shipping?                                    │ │
│ │ › What's in the box? Is it assembled?                      │ │
│ │ › What resin do you use?                                   │ │
│ │ › Do you paint miniatures?                                 │ │
│ │ › Return policy?                                           │ │
│ └────────────────────────────────────────────────────────────┘ │
└────────────────────────────────────────────────────────────────┘
│ [ STICKY ATC BAR on scroll past main CTA — już działa ]        │
└────────────────────────────────────────────────────────────────┘
```

### Mobile (< 750px) — **linear stack, zoptymalizowany**

```
┌─────────────────────────────────┐
│ [HEADER / announcement bar]     │
├─────────────────────────────────┤
│ Home · Figures · Wicked Treants │
├─────────────────────────────────┤
│ [IMAGE 1:1 full-width]          │
│ ┌──┬──┬──┬──┐                   │
│ │T1│T2│T3│T4│ swipe thumbs      │
│ └──┴──┴──┴──┘                   │
├─────────────────────────────────┤
│ PALADIN · FIGHTER               │ ← kicker
│ Wicked Treants                  │ ← Cinzel H1
│ Yilvorys Forest Monsters        │
│ ★★★★★ (127)                     │
│                                 │
│ €28.90  €12.90  [−31%]          │
│ Free EU ship €49+               │
├─────────────────────────────────┤
│ SCALE                           │
│ [28mm][32mm][75mm][128mm]       │
│                                 │
│ QTY [-][ 1 ][+]                 │
│                                 │
│ ┌─────────────────────────────┐ │
│ │ ADD TO CART · €12.90        │ │
│ └─────────────────────────────┘ │
├─────────────────────────────────┤
│ ● Ships 5–7 days                │
│ ● 12K resin · PL                │ ← trust row
│ ● 100-day return                │
├─────────────────────────────────┤
│ Product specs        [▼]        │ ← accordion
├─────────────────────────────────┤
│ Description          [▼]        │ ← accordion (collapsed)
├─────────────────────────────────┤
│ ★ Reviews (127)      [▼]        │
├─────────────────────────────────┤
│ Unlock Bonuses 🎁               │ ← stays
├─────────────────────────────────┤
│ Why IronShield [tabs swipe]     │
├─────────────────────────────────┤
│ You may also like               │
├─────────────────────────────────┤
│ FAQ                  [▼]        │
└─────────────────────────────────┘
│ [Sticky ATC bar bottom]         │
```

---

## 5 · Content mapping — co gdzie z obecnej treści

Zachowujemy **100% Twojego contentu**, tylko przestawiamy:

| Twój obecny blok | Nowe miejsce | Zmiana |
|---|---|---|
| Vendor text (DM Stash) | Kicker nad tytułem — copper, 0.18em tracking | ✅ styling only |
| Product title H1 | Cinzel Semi Bold 2.25rem | ✅ font (już fix żyje) |
| Product body_html (679 chars) | Full-width description section pod main-grid | ✅ untouched |
| Price block | Prawa kolumna pod title | ✅ restyled z DS `.ish-price` |
| Variant picker (scale) | Prawa kolumna pod price | ✅ pills czyszczone (już w DS CSS) |
| Quantity | Prawa kolumna pod variants | ✅ untouched |
| Buy buttons | Prawa kolumna — forge gradient | ✅ (już działa) |
| Judge.me preview badge | Inline z tytułem, przed ceną | ✅ reposition |
| Share button | W menu contekstowym (3-dot), nie standalone | 🔄 ukryj za toggle |
| **Subscriptions** app | Osobny accordion "Subscribe & save" pod ATC | 🔄 kolapsowany |
| **Essential estimated** delivery | Inline w trust row ("Ships by Apr 25") | 🔄 przenieś |
| **Dealeasy gift-with-product** | Accordion "Free gift with this order" pod ATC | 🔄 kolapsowany |
| Multicolumn "Unlock Bonuses" | Full-width poniżej recenzji | ✅ zostaje |
| Multirow 6 wierszy | **Zredukować do 4 tabów**: Quality · Painting · Shipping · Scale | 🔄 kompresja |
| Judge.me pełny widget | Pod description, collapsed w "Show all" | ✅ zostaje |
| Related products | Przed FAQ | ✅ zostaje |
| Dealeasy CVG progress bar | Z góry strony → **do sticky ATC bar** | 🔄 reposition |
| Selleasy upsell | Pod ATC, inline | ✅ zostaje |

### Nowe komponenty (dodane — jeszcze nie ma w contencie)

1. **Breadcrumb** (`Home · Figures · <collection> · <product>`) — Liquid z collection handle
2. **Spec table** — generowana z metafields:
   - `custom.scale` (28/32/75)
   - `custom.class` (Fighter, Paladin...)
   - `custom.race` (Human, Dragonborn...)
   - `custom.gender`
   - height / base size → jeśli w metafields, inaczej hide
3. **FAQ accordion** — 5 globalnych pytań (Shipping, Assembly, Resin, Painting service, Returns) — statyczny snippet
4. **Community gallery** (opcjonalne) — Instagram feed `#IronShieldArmy` — app required (Foursixty/Flockler) lub manualny Liquid

---

## 6 · Plan implementacji (proponowany)

### Faza A — układ 2-kolumnowy + trust row (najwyższy impact na konwersję)

Zmiana: sekcja `main-product.liquid` → split z sticky image (lewa) + sticky form (prawa). Trust row i spec table w prawej kolumnie.

**Zmiany plików:**
- `assets/ironshield-pdp.css` — nowe reguły dla 2-column grid + sticky
- `snippets/ironshield-pdp-trust.liquid` — trust row snippet (w prawej kolumnie)
- `snippets/ironshield-pdp-specs.liquid` — spec table z metafields

**Ryzyko:** Dawn używa `.product__info-wrapper` z własnym sticky logic. Może trzeba nadpisać `position: sticky` + `top` — zrobimy CSS tricks.

**Rollback:** usuń nowe reguły z `ironshield-pdp.css`, wróć do `product.legacy.json`.

### Faza B — accordion dla opisów + FAQ + sekcja tabs

**Pliki:**
- `sections/ironshield-pdp-faq.liquid` — 5 pytań (wprowadzone jako blocks)
- `sections/ironshield-pdp-tabs.liquid` — zamiast 6-wiersza multirow

Dodane do `templates/product.json` zamiast istniejącego multirow (stary `multirow_TJi9qi` zostaje w `product.legacy.json`).

### Faza C — relokacja Dealeasy progress + ukrycie mniej istotnych app blocków

Zmiana w `product.json`:
- `main` section — usunąć `subscriptions_app_block`, `dealeasy_gift_with_product` z `block_order` (te nadal na stronie ale w accordion niżej)
- Dealeasy CVG progress bar — przenieść do sticky ATC

**Ryzyko:** apps mają specific block IDs — trzeba zachować IDs przy przesuwaniu. `product.legacy.json` = instant rollback.

### Faza D — community gallery / hashtag feed (optional)

Wymaga osobnej aplikacji (Foursixty, Flockler, EmbedSocial) lub ręcznej listy URL-i do obrazów Instagram. Taniej: 6 ręcznie wybranych zdjęć w sekcji custom.

---

## 7 · Metryki — co sprawdzić po wdrożeniu

| KPI | Aktualnie | Target po zmianach |
|---|---|---|
| Checkout > Purchase (z Apr raport: 40%) | 40% | **50%+** (pasek progress + sticky ATC) |
| ATC rate | 34% (ATC/VC wg raportu) | 40%+ (ATC bliżej above-fold) |
| Avg scroll depth | ? | track z GTM |
| Time to first interaction | ? | ok. 3s (image lazy load fix) |

---

## 8 · Co już działa z DS na PDP (po ostatnich poprawkach)

- ✅ **Tytuł w Cinzel** (przez override `--font-heading-family` w scope `.product__title`)
- ✅ **ATC forge gradient**
- ✅ **Copper vendor kicker**
- ✅ **Sale price w copper**
- ✅ **Sticky ATC portal do body** (pojawia się on scroll past main ATC)
- ✅ **Focus ring copper** (WCAG AA)
- ❌ **Trust row** — usunięty na życzenie
- ❌ **2-col sticky layout** — następny krok (Faza A)
- ❌ **Spec table z metafields** — następny krok
- ❌ **FAQ accordion** — następny krok
- ❌ **Multirow → tabs** — następny krok

---

## Następny krok — czekam na Twoją decyzję

1. **Akceptujesz układ?** → ruszam Fazę A (2-col + trust row w nowym miejscu + spec table z metafields)
2. **Chcesz zmiany w proposal?** → daj znać co przesunąć/dodać/usunąć
3. **Chcesz inną hierarchię app blocków** (subscriptions/gift/shipping)? → wskaż priorytety

Wszystko jest pisane tak, żeby backup `templates/product.legacy.json` pozwolił cofnąć *każdą* zmianę w 30 sekund.
