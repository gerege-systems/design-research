[← Индекс руу буцах](README.md)

# Design Tokens

## Яагаад

Hex, px утгуудыг код даяар тараавал theme солих, брэнд өөрчлөх боломжгүй болдог. Token = утгын **нэрлэсэн давхарга**; бүх стил token-оор дамжина. Энэ файлын нэр, утга бүр `gerege-ui/packages/ui/src/styles/theme.css`-ээс хуулагдсан (2026-09-05, gerege-ui 0.12.4) — **эх сурвалж нь theme.css**, зөрвөл theme.css зөв, энд засна.

## Гурван давхаргын архитектур

```
Primitive (raw)        →  Semantic                 →  Component (заавал биш)
--color-accent-600        --accent                    --button-primary-bg
--color-neutral-100       --background-muted          --card-bg
```

1. **Primitive** — түүхий утгууд (`--color-neutral-50…950`, `--color-accent-50…950`, `--spacing-*`). Шууд UI-д хэрэглэхгүй.
2. **Semantic** — утга учиртай нэр: `--background`, `--foreground`, `--border`, `--accent`… **UI бүхэлдээ энэ давхаргаас уншина.**
3. **Component** — том системд л хэрэгтэй. gerege-ui-д байхгүй; жижиг проектод алгас.

## Semantic өнгө — light / dark бодит утгууд

Tailwind utility нэр хаалтад (`@theme inline`-аар холбогдсон).

| Token (utility) | Light | Dark | Хэрэглээ |
|---|---|---|---|
| `--background` (`bg-background`) | `hsl(0 0% 100%)` | `hsl(229 50% 6%)` | Хуудасны суурь (60%) |
| `--background-subtle` | `hsl(210 40% 98%)` | `hsl(222 47% 9%)` | Sidebar, хоёрдогч бүс |
| `--background-muted` | `hsl(210 40% 96%)` | `hsl(217 33% 13%)` | Table header, code, disabled фон |
| `--foreground` (`text-foreground`) | `hsl(222 47% 11%)` | `hsl(210 40% 98%)` | Үндсэн текст (17.9:1 / 18.8:1) |
| `--foreground-muted` | `hsl(215 19% 35%)` | `hsl(213 27% 84%)` | Хоёрдогч текст (7.4:1 / 13.3:1) |
| `--foreground-subtle` | `hsl(215 16% 45%)` | `hsl(215 20% 65%)` | Caption, placeholder (≥4.5:1 background-muted дээр ч) |
| `--border` | `hsl(214 32% 91%)` | `hsl(217 33% 17%)` | Divider, card border (чимэглэл) |
| `--border-strong` | `hsl(213 27% 84%)` | `hsl(215 25% 27%)` | Тод хил |
| `--border-input` | `hsl(215 16% 55%)` | `hsl(215 16% 45%)` | **Бүх interactive control-ийн хил** — `background` дээр 3.55:1 / 3.88:1, `background-muted` дээр ч 3.24 / 3.28 (WCAG 1.4.11 бүх фон дээр) |
| `--card` / `--card-foreground` | `hsl(0 0% 100%)` / `hsl(222 47% 11%)` | `hsl(222 47% 11%)` / `hsl(210 40% 98%)` | Card (dark-д background-subtle-аас нэг шат дээр) |
| `--popover` / `--popover-foreground` | card-тай ижил | card-тай ижил | Dropdown, popover |
| `--accent` (`bg-accent`) | `hsl(238 50% 49%)` (accent-600) | `hsl(238 60% 67%)` (accent-400) | Primary товч, линк, идэвхтэй төлөв (10%) |
| `--accent-foreground` (`text-on-accent`) | `hsl(0 0% 100%)` | `hsl(229 50% 6%)` | Accent дээрх текст (7.7:1 / 5.4:1) |
| `--accent-subtle` (`bg-accent-soft`) | `hsl(232 100% 97%)` | `hsl(238 50% 16%)` | Сонгогдсон мөр, soft badge |
| `--accent-subtle-foreground` | `hsl(238 48% 40%)` | `hsl(234 71% 78%)` | Soft фон дээрх текст |
| `--accent-hover` / `--accent-active` | `color-mix(in oklab, var(--accent) 88%/78%, black)` | `… white)` | Товчны hover/active — шинэ hex биш |
| `--secondary` / `--secondary-hover` / `--secondary-active` | `hsl(214 32% 91%)` / `hsl(213 27% 84%)` / `hsl(215 20% 76%)` | `hsl(217 33% 17%)` / `hsl(215 25% 27%)` / `hsl(215 20% 33%)` | Дүүргэсэн хоёрдогч товч. **Хүрээгүй** — дүүргэлт нь өөрөө хэлбэр; хүрээ нь `outline` вариантынх |
| `--surface-hover` / `--surface-active` | `hsl(214 32% 91%)` / `hsl(213 27% 84%)` | `hsl(217 33% 17%)` / `hsl(215 25% 27%)` | Мөр, menu item hover |
| `--overlay` | `hsl(229 50% 6% / 0.6)` | `hsl(229 50% 6% / 0.7)` | Modal backdrop |
| `--tooltip` / `--tooltip-foreground` | `hsl(222 47% 11%)` / `hsl(210 40% 98%)` | урвуу | Tooltip (inverted) |
| `--ring` | `hsl(238 55% 58%)` (accent-500) | `hsl(238 60% 67%)` (accent-400) | Focus ring |
| `--ring-offset` | `hsl(0 0% 100%)` | `hsl(229 50% 6%)` | Давхар ring-ийн дотоод давхарга |
| `--selection` / `--selection-foreground` | `var(--accent-subtle)` / `var(--accent-subtle-foreground)` | `var(--accent-subtle)` / `var(--accent-subtle-foreground)` | `::selection` — accent-ыг дагана, preset дор indigo үлдэхгүй |
| `--chart-1…6` | `hsl(217 70% 50%)`, `hsl(162 66% 29%)`, `hsl(38 92% 40%)`, `hsl(0 72% 45%)`, `hsl(215 16% 47%)`, `hsl(262 52% 55%)` | `hsl(217 80% 68%)`, `hsl(162 60% 55%)`, `hsl(38 92% 60%)`, `hsl(0 80% 68%)`, `hsl(215 20% 65%)`, `hsl(262 70% 75%)` | Categorical series — chart-1 нь accent **биш** (12-data-viz.md) |

### Статусын 4 шаттай мини scale

Статус бүр `subtle` (фон) · `border` · `foreground` (soft фон дээрх текст / бие даасан текст) · `solid` (fill) · `on-*` (solid дээрх текст):

| Статус | Light: subtle / border / foreground / solid / on | Dark: subtle / border / foreground / solid / on |
|---|---|---|
| success | `hsl(152 76% 96%)` / `hsl(152 64% 80%)` / `hsl(163 70% 26%)` / `hsl(162 66% 29%)` / цагаан | `hsl(165 80% 8%)` / `hsl(163 70% 18%)` / `hsl(154 60% 64%)` / `hsl(157 55% 50%)` / `hsl(229 50% 6%)` |
| warning | `hsl(48 100% 96%)` / `hsl(45 95% 78%)` / `hsl(23 75% 33%)` / `hsl(26 80% 40%)` / цагаан | `hsl(20 60% 10%)` / `hsl(22 70% 26%)` / `hsl(40 92% 65%)` / `hsl(35 90% 56%)` / `hsl(229 50% 6%)` |
| danger | `hsl(0 86% 97%)` / `hsl(0 96% 89%)` / `hsl(0 74% 42%)` / `hsl(0 72% 51%)` / цагаан | `hsl(0 75% 12%)` / `hsl(0 70% 35%)` / `hsl(0 91% 71%)` / `hsl(0 84% 60%)` / `hsl(229 50% 6%)` |
| info | `var(--accent-subtle)` / `hsl(233 79% 87%)` / `var(--accent-subtle-foreground)` / `var(--accent)` / `var(--accent-foreground)` | `var(--accent-subtle)` / `hsl(238 43% 32%)` / `var(--accent-subtle-foreground)` / `var(--accent)` / `var(--accent-foreground)` |

Utility нэр: `bg-success` (solid) · `text-on-success` · `bg-success-soft` · `text-success` · `border-success`. `--danger-hover`/`--danger-active` нь `--accent-hover`-той ижил `color-mix` дүрмээр.

## Өнгөнөөс гадна — бодит утгууд

**Spacing** (`--spacing-*`, rem; Tailwind `p-4` = `--spacing-4`): `0`, `px`(1px), `0_5`(2), `1`(4), `1_5`(6), `2`(8), `2_5`(10), `3`(12), `4`(16), `5`(20), `6`(24), `8`(32), `10`(40), `12`(48), `16`(64), `20`(80), `24`(96), `32`(128). Card padding 16/24 → `--spacing-4`/`--spacing-6`; 20 (`--spacing-5`) нь зөвхөн дотоод зайд, card padding-д биш.

**Radius**: `--radius-none` 0 · `--radius-sm` 4px · `--radius-md` 6px · `--radius-lg` 8px · `--radius-xl` 12px · `--radius-full` 9999px. (16px/`rounded-2xl` байхгүй — PHILOSOPHY хориотой.)

**Shadow** (neutral, 2 давхар, зөвхөн хөвөгч surface):

```css
--shadow-xs: 0 1px 2px 0 rgb(0 0 0 / 0.04);
--shadow-sm: 0 1px 3px 0 rgb(0 0 0 / 0.06), 0 1px 2px -1px rgb(0 0 0 / 0.04);
--shadow-md: 0 4px 8px -2px rgb(0 0 0 / 0.06), 0 2px 4px -2px rgb(0 0 0 / 0.04);
--shadow-lg: 0 12px 24px -6px rgb(0 0 0 / 0.08), 0 4px 8px -4px rgb(0 0 0 / 0.04);
--shadow-xl: 0 24px 48px -12px rgb(0 0 0 / 0.12), 0 8px 16px -8px rgb(0 0 0 / 0.06);
```

**Z-index** (зөвхөн token, тоо шууд бичихгүй): `--z-dropdown` 1000 · `--z-sticky` 1100 · `--z-overlay` 1200 · `--z-modal` 1300 · `--z-popover` 1400 · `--z-toast` 1500 · `--z-tooltip` 1600. Давхаргын дараалал, top-layer харилцаа — 14-app-resilience.md.

**Motion**: `--duration-fast` 120ms · `--duration-base` 160ms · `--duration-slow` 240ms; `--ease-in` `cubic-bezier(0.4,0,1,1)` · `--ease-out` `cubic-bezier(0,0,0.2,1)` · `--ease-in-out` `cubic-bezier(0.4,0,0.2,1)`.

**Breakpoints**: `--breakpoint-sm` 640 · `md` 768 · `lg` 1024 · `xl` 1280 · `2xl` 1536.

**Font**: `--font-sans` `'Geist', ui-sans-serif, system-ui, …`; `--font-mono` `'Geist Mono', ui-monospace, …`; weight зөвхөн `--font-weight-normal` 400 · `medium` 500 · `semibold` 600.

### Текстийн scale ба `--text-*--line-height` конвенц

Хэмжээ бүр **гурвалсан token**: `--text-{n}` (size) · `--text-{n}--line-height` · `--text-{n}--letter-spacing`. Tailwind v4 `text-sm` utility нь гурвууланг нь нэг дор тавьдаг — тиймээс `text-sm leading-6` гэж line-height-ийг тусад нь дарах нь scale-ээс гарч байна гэсэн үг; хэрэгтэй бол шинэ шат нэм, дарахгүй. Raw `var()` бичихдээ ч хоёуланг нь хамт: `font-size: var(--text-sm); line-height: var(--text-sm--line-height)`.

| Token | Size | Line-height | Tracking | Хэрэглээ |
|---|---|---|---|---|
| `--text-xs` | 0.75rem (12) | 1rem (16) | 0.005em | Caption, badge, table header — **доод хязгаар** |
| `--text-sm` | 0.8125rem (13) | 1.25rem (20) | 0 | UI default (товч, input, menu) |
| `--text-base` | 0.875rem (14) | 1.5rem (24) | 0 | Body (апп) |
| `--text-lg` | 1rem (16) | 1.5rem | −0.005em | Card title, landing body |
| `--text-xl` | 1.125rem (18) | 1.75rem | −0.01em | H4 |
| `--text-2xl` | 1.375rem (22) | 1.875rem | −0.015em | H3 |
| `--text-3xl` | 1.75rem (28) | 2.125rem | −0.02em | H2 / апп page title |
| `--text-4xl` | 2.25rem (36) | 2.5rem | −0.025em | H1 |
| `--text-5xl` | 3rem (48) | 1.1 | −0.03em | Landing h1 |
| `--text-6xl` | 3.75rem (60) | 1.05 | −0.035em | Hero |
| `--text-7xl` | 4.5rem (72) | 1.05 | −0.04em | Hero (том) |

Нэг бүтээгдэхүүнд эндээс **≤8** шат ашигла (02-typography.md).

## Theming — `.dark` class + pre-paint script

- Theme = `<html class="dark">` — `@custom-variant dark (&:where(.dark, .dark *))`. `data-theme` attribute, `prefers-color-scheme`-ээр token дарах хувилбар **хэрэглэхгүй** (3 төлөвийг нэг газар шийдэхийн тулд).
- Гурван төлөв: `light` / `dark` / `system` — `localStorage.theme` (байхгүй = system). Сонголт нь density-той адил төхөөрөмжийн тохиргоо тул localStorage (11-dashboard-patterns.md → Тохиргоо хаана хадгалагдах).
- `:root { color-scheme: light } .dark { color-scheme: dark }` — native control, scrollbar дагана.
- **Pre-paint script** `<head>`-д, stylesheet-ийн дараа, app bundle-аас өмнө; гадаад файл (CSP `unsafe-inline`-гүй):

```js
// /theme-init.js — gerege-ui apps/site/public
try {
  var stored = localStorage.getItem('theme');
  var prefersDark = matchMedia('(prefers-color-scheme: dark)').matches;
  var theme = stored === 'light' || stored === 'dark' ? stored : prefersDark ? 'dark' : 'light';
  if (theme === 'dark') document.documentElement.classList.add('dark');
  var accent = localStorage.getItem('brand');
  if (accent && accent !== 'default') document.documentElement.setAttribute('data-accent', accent);
} catch {}
```

- `system` төлөвт `matchMedia(...).addEventListener('change', …)` сонсож class-ыг амьд шинэчил.
- Accent preset (`data-accent="blue|violet|emerald|rose|amber"`) зөвхөн `--accent`, `--accent-subtle`, `--accent-subtle-foreground`, `--ring`-ийг сольдог (light + dark хос); hover/active нь `--accent`-ээс `color-mix`-ээр гардаг тул preset тэдгээрийг бичдэггүй (0.12.4-өөс `--color-accent-700/800`-г ч бичихээ больсон). Neutral, статус өнгө хуваалцагдана. Шинэ брэнд = шинэ preset блок, компонент код өөрчлөгдөхгүй.
- Компонент код theme мэдэхгүй — зөвхөн semantic token. Dark-д: цэвэр хар биш neutral-950; accent-600 → accent-400; shadow-ийн оронд surface шат (04-visual-details.md).

## Tailwind-тай хэрхэн зохицох

Raw token `@theme`-д, semantic нь `:root`/`.dark`-д энгийн custom property, дараа нь `@theme inline`-аар utility болгоно:

```css
@theme { --color-accent-600: hsl(238 50% 49%); --radius-md: 6px; }
:root  { --accent: var(--color-accent-600); }
.dark  { --accent: hsl(238 60% 67%); }
@theme inline { --color-accent: var(--accent); }   /* bg-accent, text-accent */
```

`@theme inline` заавал — утга нь `var()` тул theme солигдоход utility шууд дагана. shadcn/ui ч ижил загвар (2025-оос OKLCH утгатай semantic variable + `@theme inline`).

## Дүрэм

1. Компонент дотор hex/px шууд бичихгүй — заавал token.
2. Token нэр нь **юунд** хэрэглэгдэхийг хэлнэ, **ямар өнгө** болохыг биш (`--color-blue` ✗, `--accent` ✓).
3. Шинэ token нэмэхээсээ өмнө байгаагаа эргэж хар — token-ийн тоо өсөх нь системийн үнэ цэнийг бууруулдаг.
5. **Хувьсагчийн нэр ≠ утилитын нэр.** `@theme inline` нь `--color-*` гэж зарлагдсан
   нэрээр л утилит үүсгэдэг тул семантик хувьсагчийн нэрээр класс бичихэд **чимээгүй юу ч
   үүсэхгүй** — текст өв залгамжилсан өнгөөрөө үлдэж, accent дээр бараан бичвэр гарна.
   Тохирох хүснэгт:

   | CSS хувьсагч | Утилит |
   |---|---|
   | `--accent-foreground` | `text-on-accent` (0.13.2-оос `text-accent-foreground` мөн ажиллана) |
   | `--accent-subtle-foreground` | `text-on-accent-soft` (мөн `text-accent-subtle-foreground`) |
   | `--on-success` / `--on-warning` / `--on-danger` / `--on-info` | `text-on-success` … |
   | `--secondary` | `bg-secondary`, `hover:bg-secondary-hover` |

4. Контраст нь token-ийн хариуцлага: semantic хос бүр (`foreground-subtle` × `background-muted`, `border-input` × `background`, `on-*` × `*-solid`) theme.css-ийн толгойд бичсэн ratio-тай; утга солиход ratio-г дахин тооц (15-checklist.md → contrast lint). Accent бүр (preset ч, custom ч) таван хосыг 4.5:1-д барина — `accent-foreground` × `accent`, `accent` × `background`, `accent` × `background-muted`, `accent-subtle-foreground` × `accent-subtle`, `accent` × `accent-subtle`; жагсаалт нь `gerege-ui/packages/ui/src/lib/accent-pairs.ts`-д нэг газар, сангийн token тест ба showcase-ийн theme editor хоёулаа тэндээс уншина.

## W3C DTCG формат ба tooling

Token-ийг CSS-д биш **платформ-хамааралгүй JSON**-д хадгалаад Figma, CSS, iOS, Android руу build хийдэг. Стандарт: **Design Tokens Format Module 2025.10** (designtokens.org/TR/). 2025.10-аас `dimension` ба `color` утга **объект** боллоо — хуучин `"16px"`, `"#2563eb"` string хэлбэр хүчингүй:

```json
{
  "color": {
    "accent": {
      "600": {
        "$type": "color",
        "$value": { "colorSpace": "srgb", "components": [0.306, 0.373, 0.769], "hex": "#4e5fc4", "alpha": 1 }
      }
    },
    "action": {
      "$type": "color",
      "$value": "{color.accent.600}",
      "$description": "Primary action, links (10%)"
    }
  },
  "spacing": {
    "4": { "$type": "dimension", "$value": { "value": 16, "unit": "px" } }
  },
  "duration": {
    "base": { "$type": "duration", "$value": { "value": 160, "unit": "ms" } }
  }
}
```

- `$value`, `$type` заавал; `$description`, `$deprecated` (boolean эсвэл шалтгааны string), `$extensions` (reverse-domain түлхүүр: `"mn.gecore.figma"`) сонголт. Group = `$value`-гүй объект; `$type`-ыг group дээр тавьбал доторх token-ууд өвлөнө.
- Alias `{color.accent.600}` = primitive → semantic холбоос файлд өөрт нь шингэнэ.
- `$type`-ууд: `color`, `dimension`, `fontFamily`, `fontWeight`, `duration`, `cubicBezier`, `number`, `shadow`, `typography`, `border`, `gradient`, `transition` (composite).
- **Build**: Style Dictionary v4/v5 (DTCG-г шууд уншина) → `tokens.css` (`:root { --accent: … }` + `.dark { … }`), `.ts`, Swift, Kotlin. Tokens Studio (Figma plugin) тэр JSON-ийг Figma variables ↔ git хоёр тийш sync → **Figma ба код нэг эх сурвалжтай**.
- Theme = тусдаа set: `tokens/core.json` (primitive) + `tokens/light.json` + `tokens/dark.json` (semantic alias л өөр). Build нь `:root` ба `.dark` блок хоёрыг гаргана.
- Нэрлэлтийн давхарга JSON зам болно: `color.accent.600` (primitive) → `color.action` (semantic) → `button.primary.bg` (component). CSS var нэр = зам `-`-ээр.
- **Хувилбар** (`@org/tokens`, semver): token нэр устгах/солих = **major**; шинэ token нэмэх = **minor**; утга өөрчлөх = **patch**, гэхдээ харагдац мэдэгдэхүйц өөрчлөгдвөл (accent hue, radius scale) = **minor + CHANGELOG-д «visual breaking» тэмдэглэл**. Устгахаасаа өмнө нэг хувилбар `$deprecated` тэмдэглэ. Changesets-ээр CHANGELOG автоматаар.
- Жижиг проект (нэг платформ, нэг repo): JSON давхарга шаардлагагүй — theme.css шиг шууд CSS var хангалттай. DTCG нь Figma + 2-оос олон платформ байхад л үнэ цэнтэй.

## Эх сурвалж

- gerege-ui — `packages/ui/src/styles/theme.css`, `apps/site/public/theme-init.js`, `packages/ui/docs/PHILOSOPHY.md` — github.com/gerege-systems/gerege-ui
- W3C Design Tokens Community Group — Design Tokens Format Module 2025.10 — designtokens.org/TR/drafts/format/ (2026-08-21 шалгасан)
- Style Dictionary — DTCG support — styledictionary.com
- Tokens Studio for Figma — docs.tokens.studio
- Tailwind CSS v4 — Theme variables (`@theme`, `@theme inline`), `@custom-variant` — tailwindcss.com/docs/theme
- shadcn/ui — Theming (OKLCH semantic CSS variables) — ui.shadcn.com/docs/theming
- MDN — Using CSS custom properties; `color-scheme`; `color-mix()`
- Material 3 — Design tokens overview — m3.material.io/foundations/design-tokens
- Changesets — github.com/changesets/changesets
- Nathan Curtis (EightShapes) — «Naming Tokens in Design Systems»
