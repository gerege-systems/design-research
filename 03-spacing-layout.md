[← Индекс руу буцах](README.md)

# Spacing ба Layout

## Spacing scale — 4px/8px систем

Зайг дур мэдэн биш тогтмол шатлалаас сонгоно:

```
4 · 8 · 12 · 16 · 24 · 32 · 48 · 64 · 96 · 128
```

- Жижиг зай (component дотор): 4/8/12/16
- Дунд зай (component хооронд): 24/32
- Том зай (section хооронд): 48/64/96+
- Tailwind-ийн spacing scale (`p-1`=4px, `p-2`=8px…) яг энэ систем.

**Зарчим**: холбоотой зүйлс ойр, холбоогүй нь хол (proximity). Гарчиг нь дараагийн контентдоо өмнөх section-ээсээ илүү ойр байх ёстой — `margin-top > margin-bottom`.

## Container ба хуудасны өргөн

| Хэрэглээ | Max-width |
|---|---|
| Урт текст (prose, docs) | ≤65ch |
| Форм, settings, текст хуудас | 720px |
| Landing / мэдээний сайт container | 1152-1280px |
| Dashboard content | fluid, дээд тал нь 1536px (table/chart бүтэн өргөн) |

Auth card (400-440px) нь хуудас биш card — 11-dashboard-patterns.md-г үз.

Хажуугийн padding: mobile 16px, tablet 24px, desktop 32px+.

## Breakpoints

Түгээмэл жишиг (Tailwind):

```
sm: 640px · md: 768px · lg: 1024px · xl: 1280px · 2xl: 1536px
```

- **Mobile-first**: суурь стилиэ жижиг дэлгэцэд бичээд `min-width` query-гээр өргөжүүл.
- Breakpoint-ыг төхөөрөмжөөр биш **контентоор** сонго — layout эвдэрч эхэлсэн цэг дээр л breakpoint тавь.
- 3-4 breakpoint ихэнх сайтад хүрэлцдэг; бүх breakpoint бүрд бүх зүйлийг өөрчлөх шаардлагагүй.

## Grid

- **12 багана** — сонгодог, уян хатан (2/3/4/6 хуваагдана).
- CSS Grid + `gap` — margin-аар зай барихаас илүү цэвэр.
- Card grid-д: `grid-template-columns: repeat(auto-fill, minmax(280px, 1fr))` — breakpoint бичилгүйгээр өөрөө responsive болно.
- Gutter: mobile 16px, desktop 24-32px.

## Responsive зарчмууд

- Хэвтээ scroll хэзээ ч үүсгэхгүй — өргөн контент (table, code) өөрийн `overflow-x: auto` саванд.
- Зураг: `max-width: 100%; height: auto` + `aspect-ratio`-оор layout shift-ээс сэргийл.
- Sidebar → mobile-д drawer/bottom nav болдог; table → card эсвэл хэвтээ scroll.
- Pointer target: desktop/нягт dashboard-д **≥24×24px** (WCAG 2.2 AA); touch-first дэлгэц, landing CTA-д **≥44×44px**; хөрш target хооронд ≥8px.
- Hover-т тулгуурласан UI mobile-д ажиллахгүй — чухал үйлдлийг hover-ийн цаана бүү нуу.

## Мэдэх ёстой нюансууд

- **8px grid-ээс гарах нь алдаа биш** — optical alignment (нүдэнд зөв харагдах) нь математик alignment-аас чухал. Icon текстийн хажууд байрлахдаа 1-2px шилжих нь хэвийн.
- Vertical rhythm: line-height + margin нийлээд тогтмол хэмнэл үүсгэвэл хуудас «амьсгалтай» харагдана.
- Whitespace бол feature — багтаах гэж шахахаар хуудас хямд харагддаг. Эргэлзвэл зайг нэм.

## Орчин үеийн CSS layout

- **Container query компонентэд, media query хуудсанд.** Card, widget, sidebar item өөрийн савны өргөнөөс хамаарч хувирна — хуудас хаана тавьснаас биш:

```css
.card-list { container-type: inline-size; }
@container (min-width: 40rem) { .card { grid-template-columns: 1fr 2fr; } }
```

`container-type: inline-size` тавьсан элементийн өргөн нь дотоод контентоос хамаарахаа болино — grid/flex child дээр тавихдаа `min-width: 0` шалга.

- **`vh`-д суурилсан бүтэн өндөр хориотой** (Tailwind `h-screen`/`min-h-screen` ч мөн) — mobile-д URL bar нуугдах/гарахад утга хувьсаж доод хэсэг тасарна. Бүтэн дэлгэцийн layout: `min-height: 100dvh` (динамик); header/hero-д хөдөлгөөнгүй байлгах бол `100svh`. `dvh`-г бүх зорилтот browser дэмждэг (2023+) — fallback хэрэггүй.
- **Safe area**: `<meta name="viewport" content="width=device-width, viewport-fit=cover">` + fixed элементэд `padding-bottom: max(16px, env(safe-area-inset-bottom))`. Bottom nav, toast, FAB-д заавал.
- Hover-only affordance (hover-д гарч ирэх товч, tooltip)-ийг **`@media (hover: hover) and (pointer: fine)`**-аар хааж, touch-д үргэлж харагдуулна. `:hover` стиль л биш — туршилтын логик ч үүгээр салга.
- `:has()` — JS-гүй layout төлөв: `.form:has(:invalid) .submit { opacity: .5 }`, `.layout:has(.sidebar[data-open])`, `.card:has(img)` гэх мэт. Сонгуур бүрийн зардал өндөр тул `body:has(...)` маягийн өргөн хүрээтэй хэрэглээг 2-3-аас хэтрүүлэхгүй.
- **Logical properties**: `margin-left/right` биш `margin-inline`, `padding-top/bottom` биш `padding-block`, `width` биш `inline-size`, `text-align: left` биш `start`. RTL-д шууд ажиллана; шинэ кодод physical property бичихгүй.
- **Subgrid** — card grid дотор гарчиг/тайлбар/товч нь хөрш card-уудтайгаа мөр мөрөөрөө тэгшлэгдэнэ:

```css
.grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); }
.card { display: grid; grid-row: span 3; grid-template-rows: subgrid; }
```

## Эх сурвалж

- WCAG 2.2 — SC 1.4.10 Reflow, 2.5.8 Target Size (Minimum), 1.3.4 Orientation — w3.org/TR/WCAG22/
- MDN — CSS container queries; `dvh`/`svh`/`lvh` units; `env()`; `:has()`; CSS logical properties; Subgrid — developer.mozilla.org/en-US/docs/Web/CSS/
- web.dev — «The large, small, and dynamic viewport units»; «Container queries land in stable browsers»; «Learn CSS: Grid, Spacing»
- Tailwind CSS docs — Responsive design (breakpoints), Spacing scale — tailwindcss.com/docs
- Material 3 — Layout: Window size classes; Apple HIG — Layout
- Refactoring UI — «Establish a spacing and sizing system», «Start with too much white space»
- Nielsen Norman Group — «Proximity Principle in Visual Design»
