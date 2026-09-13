[← Индекс руу буцах](README.md)

# Өнгө

## 60-30-10 дүрэм

Хамгийн түгээмэл, найдвартай жишиг:

- **60% — суурь өнгө** (background, surface): цайвар neutral эсвэл dark theme-д saturation багатай харанхуй өнгө. Хуудасны «агаар».
- **30% — хоёрдогч өнгө** (card, sidebar, header, border, muted text): суурьтайгаа ойролцоо hue боловч ялгарах түвшний өнгө.
- **10% — accent** (CTA товч, линк, идэвхтэй төлөв, badge): брэндийн гол өнгө. Цөөн хэрэглэх тусмаа хүчтэй — бүх юм accent болбол юу ч онцгойрохгүй.

Accent-ийн **зөвшөөрөх / хориглох** жагсаалт (нэг view-д accent-fill ≤1 элемент):

| Accent зөвшөөрнө | Accent хориглоно |
|---|---|
| Primary товч (view-д 1) | Secondary/tertiary товч, icon button |
| Линк текст, идэвхтэй nav/tab индикатор | Гарчиг, body текст, label |
| Focus ring, сонгогдсон checkbox/radio/switch | Card, section, sidebar фон (accent-subtle ч биш) |
| Progress/loading bar, идэвхтэй chart highlight | Chart series-ийн өнгө (12-data-viz.md), status badge |
| Notification dot, шинэ зүйлийн badge (жижиг) | Border, divider, shadow, gradient |

Mobile дээр 30%-ийн давхарга (sidebar, panel) багасаж 60% давамгайлдаг — accent-ээ mobile дээр ч 10%-иас хэтрүүлэхгүй, CTA-гаа эрэмбэлэх.

## Palette-ийн бүтэц

Бодит UI-д өнгө 3-хан биш:

- **Neutral scale** — 8-10 шатлал (50→900). Slate/zinc/gray маягийн нэг neutral сонгоод бүх фон, текст, border-ыг үүнээс гаргана.
- **Accent scale** — брэндийн өнгөний 50→900 шатлал.
- **Semantic** — success (ногоон) / warning (шар) / error (улаан), заримдаа info (цэнхэр). Тус бүр цөөн шатлалтай байхад хангалттай.

Гэхдээ **hue-гийн тоо** нь 1 accent + 2-3 semantic-аас хэтрэхгүй байх нь цэвэрхэн харагдана.

## Contrast

- Энгийн текст (<24px): фонтойгоо **≥4.5:1** (WCAG AA)
- Том текст (≥24px, эсвэл ≥18.7px bold): **≥3:1**
- Accent өнгөн дээрх цагаан текст ихэвчлэн энд унадаг — Tailwind-ийн 500 биш **600**-г товчны фон болгох нь аюулгүй.
- UI компонентын хил (input border, icon): non-text contrast **≥3:1**. Gerege-ийн form control-ийн хил зориуд зөөлөн — үл хамаарах зүйлийг 04-visual-details.md-ээс үз.

## Dark mode

- Өнгийг hex-ээр биш **semantic token**-оор (`--background`, `--card`, `--accent` — нэрс 08-design-tokens.md) тодорхойлбол 60-30-10 харьцаа хоёр theme-д хоёуланд ажиллана.
- Dark theme-д accent-ийн **chroma −10–20%, lightness +0.10–0.15** (OKLCH L). theme.css-ийн бодит алхам: light `--accent` = accent-600 (L≈0.53) → dark `--accent` = accent-400 (L≈0.66); accent дээрх текст dark-д цагаан биш neutral-950 (`--accent-foreground`), контраст 5.4:1.
- Dark фон нь цэвэр хар (#000) биш, neutral-ийн 900-950 түвшин (#09090b, #0f172a гэх мэт).
- Elevation-ыг dark theme-д shadow биш **surface-ийн цайралтаар** илэрхийлдэг (дээшлэх тусам жаахан цайвар).

## Практик санамж

- Ижил lightness-тэй өнгөнүүд hue өөр байсан ч зэрэгцэхэд «вибрация» үүсгэдэг — саарал дээр saturated өнгө тавихдаа lightness-ийн зөрүү өг.
- Ногоон/улааныг статусын цорын ганц ялгаа болгохгүй — өнгөний хараа сулруу хүнд icon/label давхар өг.
- Брэнд тогтоогүй проектод: neutral суурь (slate/zinc) + нэг accent + semantic 3 өнгөөр эхэл — дараа нь accent token-оо л солино.

## color-scheme ба системийн горимууд

- **`:root { color-scheme: light dark; }` заавал** — browser-ийн native form control, scrollbar, `<select>` попап theme-ээ дагана. Үгүй бол dark theme дээр цагаан scrollbar, цагаан checkbox үлддэг.
- Гараар сольж байгаа бол `:root { color-scheme: light } .dark { color-scheme: dark }` — `.dark` class нь pre-paint script-ээр тавигдана (08-design-tokens.md-г үз). `data-theme` хэрэглэхгүй.
- `@media (prefers-contrast: more)` — border-ыг 1px→2px, `--foreground-muted`-ийг `--foreground` руу ойртуул, shadow-г border-оор соль.
- `@media (forced-colors: active)` (Windows High Contrast): өнгөний токен бүгд хүчингүй болж **системийн өнгө** (`Canvas`, `CanvasText`, `ButtonText`, `Highlight`, `LinkText`) үйлчилнэ. Дүрэм: `background`-аар илэрхийлсэн хил, сонгогдсон төлөв алга болдог тул `border: 1px solid transparent`-ийг урьдчилж бич — forced mode-д transparent нь системийн өнгө болно. Зайлшгүй өнгө хадгалах (брэнд лого, статус цэг) элементэд л `forced-color-adjust: none`.
- `@media (prefers-reduced-transparency: reduce)` — `backdrop-filter: blur()`, `opacity < 1` фонг тунгалаг бус surface-аар соль.

## OKLCH ба орчин үеийн палитр

- **OKLCH = perceptual uniform**: L (0-1) нэгийг өөрчлөхөд нүдэнд харагдах цайралт hue бүрд ижил өөрчлөгддөг (HSL-д шар L=50% нь цэнхэр L=50%-аас хавьгүй цайвар харагддаг). Tailwind v4-ийн default palette бүхэлдээ OKLCH.
- Scale гаргах дүрэм: **C ба H тогтмол, зөвхөн L-ийг өөрчил**. Жишээ 50→900: L = 0.97 · 0.93 · 0.87 · 0.78 · 0.68 · 0.58 · 0.50 · 0.42 · 0.34 · 0.26. Accent-ийн C **≤0.2** (брэнд зайлшгүй шаардвал л илүү — тэгвэл P3 fallback-ийг заавал шалга); захын шатлалд (50/900) C-г 30-50% бууруул — эс бөгөөс gamut-аас гарна. Санамж: Tailwind blue-600 `#2563eb` нь C≈0.21 — cap-аас дээш.
- Hover/tint-ийг шинэ hex биш `color-mix`-ээр: `color-mix(in oklch, var(--accent), black 10%)` (hover), `color-mix(in oklch, var(--accent), transparent 90%)` (цайвар фон). Interpolation-д `in oklch` — `in srgb` дунд нь бохир саарал үүсгэдэг.
- **P3 gamut**: OKLCH нь sRGB-ээс гадна өнгө (C > ~0.3) заах боломжтой; sRGB дэлгэцэнд browser өөрөө clip хийнэ. Хуучин browser-т fallback давхар бичих: `background: #4e5fc4; background: oklch(0.53 0.16 273);` (gerege-ui accent-600 — бодит утга, cap дотор), эсвэл `@supports (color: oklch(0% 0 0))`.
- **APCA** (WCAG 3 drafts): харьцаа биш Lc утга, текстийн хэмжээ/weight-тэй уялдана. Ойролцоо босго: body текст (14-16px/400) **Lc ≥ 75**, том текст (24px+/700) **Lc ≥ 60**, placeholder/disabled Lc ≥ 45. Одоогоор хуулийн шаардлага нь WCAG 2.x ratio хэвээр — APCA-г **нэмэлт** шалгуур болгож, ratio-г ч хангаж бай.

## Эх сурвалж

- WCAG 2.2 — SC 1.4.3 Contrast (Minimum), 1.4.6 Contrast (Enhanced), 1.4.11 Non-text Contrast, 1.4.1 Use of Color — w3.org/TR/WCAG22/
- MDN — `color-scheme`, `forced-colors`, `forced-color-adjust`, `prefers-contrast`, `prefers-reduced-transparency`, `color-mix()`, `oklch()` — developer.mozilla.org/en-US/docs/Web/CSS/
- web.dev — «Styling for Windows high contrast with new standards for forced colors»; «Building a color scheme»
- Evil Martians — «OKLCH in CSS: why we moved from RGB and HSL» — evilmartians.com/chronicles/oklch-in-css-why-quit-rgb-hsl
- Tailwind CSS v4 docs — Colors (OKLCH palette) — tailwindcss.com/docs/colors
- APCA Readability Criterion — apcacontrast.com; W3C Silver/WCAG 3 Working Draft
- Refactoring UI (Wathan, Schoger) — «Building Your Color Palette»
- Apple HIG — Color; Material 3 — Color system
