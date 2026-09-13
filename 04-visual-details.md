[← Индекс руу буцах](README.md)

# Visual нарийн ширийн — radius, shadow, border, icon

## Border radius

Нэг проектод 3-4 л утга байхад хангалттай:

| Хэрэглээ | Утга |
|---|---|
| `--radius-sm` — badge, checkbox, tag | 4px |
| `--radius-md` — товч, input, select | 6px |
| `--radius-lg` — card, dropdown, popover | 8px |
| `--radius-xl` — modal, drawer, том card | 12px |
| `--radius-full` — avatar, pill | 9999px |

- **Дотоод radius = гадаад radius − padding**: card 12px radius, padding 8px бол доторх зураг 4px radius байж нүдэнд зөв харагдана. Ижил утга давхарлавал дотоод булан «бүдүүн» харагддаг.
- Нэг брэнд-шийдвэр: sharp (0-4px, «ноцтой» ERP/fintech маяг) vs rounded (8-16px, найрсаг SaaS маяг) — хольж болохгүй.

## Shadow / Elevation

Shadow нь «гоёл» биш **давхаргын мэдээлэл** — юу юуны дээр байгааг хэлдэг:

| Түвшин | Хэрэглээ | Жишээ |
|---|---|---|
| `--shadow-xs` | (бараг хэрэглэхгүй) | `0 1px 2px 0 rgb(0 0 0 / 0.04)` |
| `--shadow-sm` | Hover-той card | `0 1px 3px 0 rgb(0 0 0 / 0.06), 0 1px 2px -1px rgb(0 0 0 / 0.04)` |
| `--shadow-md` | Dropdown, popover | `0 4px 8px -2px rgb(0 0 0 / 0.06), 0 2px 4px -2px rgb(0 0 0 / 0.04)` |
| `--shadow-lg` | Drawer, command palette | `0 12px 24px -6px rgb(0 0 0 / 0.08), 0 4px 8px -4px rgb(0 0 0 / 0.04)` |
| `--shadow-xl` | Том marketing overlay (modal = lg) | `0 24px 48px -12px rgb(0 0 0 / 0.12), 0 8px 16px -8px rgb(0 0 0 / 0.06)` |

- Shadow **зөвхөн хөвөгч (floating) surface-д** — тайван card, товч, input-д shadow биш 1px border.
- Чанартай shadow нь **2 давхар**: нэг жижиг тод (contact) + нэг том бүдэг (ambient) — дээрх утгууд яг ийм.
- Гэрэл **дээрээс** тусдаг гэж үзнэ — y-offset эерэг, x-offset 0.
- **Neutral хар** (`rgb(0 0 0 / α)`), hue tint-гүй — өнгөт сүүдэр PHILOSOPHY-ийн хориотой жагсаалтад.
- **Dark mode-д shadow бараг ажилладаггүй** — elevation-ыг surface-ийн алхмаар гарга (theme.css-ийн бодит шат): `--background` (L 6%) → `--background-subtle` (9%) → `--card`/`--popover` (11%) → `--background-muted` (13%); хөвөгч surface-д дээр нь `1px solid var(--border)`. Shadow-г dark-д үлдээж болно, гэхдээ border нь гол ялгагч.
- Border vs shadow: data-нягт UI-д 1px border хямдхан бөгөөд цэвэр; shadow-г interactive/floating элементэд үлдээ.

## Border ба divider

- Өнгө: neutral scale-ийн 200 (light) / 800 (dark) орчим — текстээс хамаагүй сул.
- 1px хангалттай; 2px нь зөвхөн focus/selected төлөвт.
- Divider-ийг spacing-аар орлуулж болдог — зай хангалттай бол шугам хэрэггүй. Шугам их = чимээ их.
- Input border нь WCAG-аар non-text contrast 3:1 шаардлагад унадаг. **Gerege-ийн шийдвэр (2026-09-13, 09-14-нд шинэчилсэн):** form control болон outline товчны хил card-ын хилтэй ижил (`--border-input` = `--border`, цагаан дээр 1.24:1) — 1.4.11-ийг хангахгүй гэдгийг мэдэж сонгосон; `prefers-contrast: more` үед 3:1 руу буцна. Үүнээс цааш цайруулахгүй, талбарыг label-гүй бүү үлдээ.

## Icon

- Нэг л icon set хэрэглэ (Lucide, Heroicons, Phosphor…) — өөр set-ийн icon stroke/style-аараа зөрдөг.
- Хэмжээ текстийн хэмжээгээр: 12–13px текст → **14px** icon · 14px → **16px** · 16px → **18–20px** · 18–20px → **20px** · 24px+ гарчиг → **24px**. Icon-only товчинд 16px icon + 36px товч (дотор 24px hit target хангана). Lucide default stroke 2px; 14px-д 1.75px хүртэл нимгэлж болно.
- Stroke width тогтмол (ихэвчлэн 1.5-2px); icon-ыг scale хийвэл stroke зузаан/нимгэн болдгийг анзаар.
- Icon + text хослохдоо `gap: 8px`, вертикаль төвлөрөл `align-items: center`.
- Icon-only товчид заавал `aria-label`.

## Зураг ба media

- `aspect-ratio` заавал өг — layout shift (CLS) -ээс сэргийлнэ.
- `object-fit: cover` + төвлөрөл; хүний зурагт `object-position` анхаар.
- Placeholder: blur-up эсвэл dominant-color фон — цагаан гялсхийлт муухай.
- Format: WebP/AVIF, srcset-тэй responsive; hero зургийг preload, доод зургуудыг lazy.

## Эх сурвалж

- Refactoring UI (Wathan, Schoger) — shadow давхарлах, border vs shadow, дотоод/гадаад radius харьцаа.
- Material Design 3 — Elevation (m3.material.io/styles/elevation), Icons (stroke/size).
- Apple HIG — Materials, Icons, Images.
- WCAG 2.2 SC 1.4.11 Non-text Contrast (3:1 — input border, icon).
- web.dev — Optimize Cumulative Layout Shift (`aspect-ratio`, зургийн хэмжээ), Serve responsive images, Image formats (WebP/AVIF).
- MDN — `object-fit`, `object-position`, `aspect-ratio`, `<img srcset>`.
- Lucide / Heroicons / Phosphor баримт — icon хэмжээ, stroke width.
