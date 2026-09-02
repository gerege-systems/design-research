# Design Research

Вэб дизайны нарийн ширийн жишиг, стандарт, практик дүрмүүдийн нэгдсэн тэмдэглэл. Responsive вэбсайт, SaaS/dashboard UI, landing page-д хамаарна. Тоон утга бүр [00-defaults.md](00-defaults.md) ба `gerege-ui` theme.css-тэй ижил.

## Агуулга

| Файл | Сэдэв |
|---|---|
| [00-defaults.md](00-defaults.md) | **Эхлэх цэг** — муж бүрээс нэг default; бодолгүй хэрэглэх хүснэгт |
| [01-color.md](01-color.md) | Өнгөний харьцаа — 60-30-10, accent allow/deny, palette, OKLCH, dark mode |
| [02-typography.md](02-typography.md) | Geist, type scale (1.2@14 / 1.25@16), rem, clamp, шрифт ачаалалт (≤4 woff2) |
| [03-spacing-layout.md](03-spacing-layout.md) | 4/8 scale, container (1536/720/1280/65ch), breakpoint, `dvh`, container query |
| [04-visual-details.md](04-visual-details.md) | Radius 4/6/8/12, neutral shadow, dark elevation шат, icon хэмжээ текстээр |
| [05-motion.md](05-motion.md) | 120/160/240ms token, easing, reduced motion, View Transitions |
| [06-components.md](06-components.md) | Компонентын **анатоми**: товч (32/36/40/44), форм (validation, input матриц, OTP, mask…), table, 5 төлөв, modal, toast, tabs… |
| [07-accessibility.md](07-accessibility.md) | WCAG 2.2, contrast бүх фон дээр, focus ring (давхар), pointer target 24/44, APCA |
| [08-design-tokens.md](08-design-tokens.md) | theme.css-ийн бодит light/dark утгууд, `--text-*--line-height`, `.dark` + pre-paint, DTCG 2025.10 |
| [09-localization-mn.md](09-localization-mn.md) | Монгол: кирилл шрифт, overflow хүснэгт, огноо `yyyy-MM-dd HH:mm`, `25,000₮`, хэлний хадгалалт |
| [10-ux-writing-mn.md](10-ux-writing-mn.md) | UX бичвэр монголоор: sentence case, товчны үйл үг, дагавар/ICU plural, алдааны томьёо, нэгж |
| [11-dashboard-patterns.md](11-dashboard-patterns.md) | Dashboard/ERP **зан төлөв**: app shell, prefs хадгалах, table, search, pagination, destructive/Undo, permission, keyboard |
| [12-data-viz.md](12-data-viz.md) | Chart сонголт, KPI tile, `--chart-1…6`, axis, Recharts/ECharts, a11y |
| [13-landing.md](13-landing.md) | Landing: hero, section хэмнэл, CTA xl, pricing, SEO, Core Web Vitals |
| [14-app-resilience.md](14-app-resilience.md) | 404/403/500, offline, session, 429, stale, z-index/top-layer/scroll-lock, print, SPA route, bottom bar/PWA |
| [15-checklist.md](15-checklist.md) | Pre-ship шалгах хуудас + dashboard/chart/landing gate + CI автоматжуулалт |
| [16-sources.md](16-sources.md) | Бүх эх сурвалж + давамгайллын дүрэм |
| [references/](references/README.md) | Амтны сан — зорьж буй / зайлсхийх жишиг зургууд |

## Давамгайлал ба эзэмшил

1. **Pattern файл (11 dashboard, 12 data-viz, 13 landing) өөрийн домэйнд суурь файл (01–08)-ыг дарна.**
2. **Сэдэв бүр нэг л эзэн файлтай**; бусад файл холбоос өгнө, давтахгүй. Компонентын анатоми = 06; dashboard-ын зан төлөв (search, pagination, confirmation, keyboard) = 10; формат (огноо/₮/утас) = 09; бичвэр = 16; token утга = 08.
3. Тоо зөрвөл: `gerege-ui` theme.css = 00-defaults = энэ README > бусад файл. Зөрүү олдвол эзэн файлыг засна.

## Товч дүрмүүд (cheat sheet)

- **Өнгө**: 60% суурь · 30% хоёрдогч · 10% accent; 1 accent hue (C ≤0.2), accent-fill view-д 1; semantic 4 (success/warning/danger/info).
- **Font**: Geist (cyrillic-ext) + mono · ≤8 size · weight 400/500/600 · ≤4 woff2. Апп 1.2 @ 14px, маркетинг 1.25 @ 16px; min 12px (chart tick 11).
- **Нэгж**: текстэд rem (`--text-*` + `--text-*--line-height` хосоор), зайд 4/8 scale; өндөрт `dvh`.
- **Хэмжээ**: товч sm 32 / md 36 / lg 40 / xl 44; table мөр 36 / 44 / 52; card padding 16 / 24; radius 4 / 6 / 8 / 12.
- **Max-width**: dashboard fluid ≤1536 · форм/settings/текст 720 · landing 1152–1280 · prose 65ch.
- **Contrast**: текст ≥4.5:1; UI/border/icon ≥3:1 **бүх фон дээр**. Focus ring 2px + offset 2px, accent-fill дээр давхар ring.
- **Pointer target**: desktop/нягт dashboard 24px; touch-first/landing CTA 44px; хооронд ≥8px.
- **Motion**: 120 / 160 / 240ms token, ease-out, transform+opacity; `prefers-reduced-motion` хүндэл.
- **Token**: hex-ийг шууд бүү хэрэглэ — `--background`, `--foreground`, `--border-input`, `--accent`…; theme `.dark` class + pre-paint; z-index `--z-*` л.
- **Хариу**: 100ms шууд · 1s урсгал · 10s анхаарал; skeleton 300ms хойшлуул; tooltip 500ms.
- **Төлөв**: loading / empty (first-run | filtered) / error / success / permission-denied — 5.
- **Destructive**: Undo 5с (bulk 10с) · modal confirm · type-to-confirm; товч = үйлдлийн үг.
- **Validation**: blur дараа, дараа нь input бүрт; эхний keystroke хэзээ ч биш; >5 алдаа → error summary.
- **Prefs**: density/sidebar/theme → localStorage · filter/sort/page/q → URL · аккаунт → сервер. Хэл: public URL `/mn/`, апп сервер+cookie.
- **Монгол**: `Өө Үү` шрифтэд бий эсэх; товч `min-width`; огноо `yyyy-MM-dd HH:mm` UTC+8, Даваагаас; `1,250,000₮` suffix; `+976 XXXX XXXX`; sentence case; дагаварыг динамик нэрэнд залгахгүй.
- **Chart**: bar 0-ээс; ≤6 `--chart-*` hue, accent биш; pie ≤5; Recharts (≤1k цэг) / ECharts; өнгө дангаар мэдээлэл дамжуулахгүй.
- **Landing**: 1 h1 · 1 primary CTA (xl) · LCP ≤2.5s · JS ≤150KB gz · 320/375/768/1280/1920.

## Хэрэгжүүлэлт

Эдгээр дүрмийг хэрэгжүүлсэн компонентын сан: [`@gerege-systems/ui`](https://github.com/gerege-systems/gerege-ui) — showcase https://ui.gecore.mn. Дүрэм энд, код тэнд; token утгын эх сурвалж `packages/ui/src/styles/theme.css`.

## Ашиглах урсгал

1. `00-defaults.md` — утгаа аваад эхэл.
2. `references/` — зорьж буй амтыг зургаар хар.
3. Хэв маяг: dashboard бол `11`, landing бол `13`, chart бол `12`; апп бүрт `14`.
4. Суурь дүрэм `01–08` + монгол `09` + бичвэр `10`.
5. Мокап → батлуул → код.
6. Дуусахын өмнө `15-checklist.md`-ийг бүхэлд нь (хуудасны төрлийн gate-тэй) ажиллуул.
