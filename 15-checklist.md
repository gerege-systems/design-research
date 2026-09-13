[← Индекс руу буцах](README.md)

# Pre-ship шалгах хуудас

## Яагаад

Дүрмүүд 01–12 файлд (default утга [00-defaults.md](00-defaults.md)-д) бий; энэ нь тэдгээрийг **хуудас/компонент бүр дээр нэг удаа, дараалалтай** давах хуудас. Бүх `[ ]` тэмдэглэгдээгүй бол «дууссан» гэж хэлэхгүй. Claude ч, хүн ч ижил жагсаалтаар явна. Тоо энд 00-defaults.md-тэй зөрвөл 14 зөв.

## Төлөвүүд

- [ ] loading / empty (first-run **ба** filtered) / error / success / permission-denied — **5 төлөв** бүгд хэрэгжсэн (06-components.md)
- [ ] permission-denied нь хоосон хуудас биш — «эрх хүрэхгүй» + хэн өгөх + буцах зам (11-dashboard-patterns.md)
- [ ] skeleton 300ms хойшлогдож, эцсийн layout-тай ижил хэлбэр, өндөртэй — контент ирэхэд үсрэлт 0
- [ ] empty state-д юу байхгүй + CTA товч байна; filtered хоосонд header/chip үлдсэн
- [ ] error төлөвд retry эсвэл дараагийн алхам бий; техник код хэрэглэгчид харагдахгүй
- [ ] **Loading үед товч/талбарын хэмжээ өөрчлөгдөхгүй, давхар submit боломжгүй**

## Өнгө & contrast

- [ ] Энгийн текст фонтойгоо ≥4.5:1 (01-color.md, 07-accessibility.md)
- [ ] Том текст, icon ≥3:1 — **бүх фон дээр** (background, background-muted, card); `--border-input` зориуд зөөлөн, харин `prefers-contrast: more` дээр ≥3:1 (07-accessibility.md)
- [ ] Accent hue 1 л байна; accent-fill элемент view-д 1; chart-д accent биш `--chart-*` (01-color.md, 11)
- [ ] Статус өнгөөр дангаар биш — icon эсвэл label давхар (07-accessibility.md)
- [ ] Dark mode (`.dark`)-д бүх хуудас нээж үзсэн; hex биш semantic token; `data-theme` байхгүй (08-design-tokens.md)
- [ ] **Focus ring хоёр theme-д хоёуланд харагдана: 2px outline + 2px offset; accent-fill дээр давхар ring** (07-accessibility.md)
- [ ] Shadow зөвхөн хөвөгч surface, neutral; gradient/glass/өнгөт сүүдэр байхгүй (04-visual-details.md)

## Типограф

- [ ] font-size бүгд rem, `--text-*` + `--text-*--line-height` хосоор; `62.5%` трюк байхгүй (02, 08)
- [ ] Mobile-д input/select/textarea ≥16px — iOS zoom үүсэхгүй (06-components.md)
- [ ] Хамгийн жижиг текст 12px (chart tick 11 зөвшөөрнө); нийт font-size ≤8, weight 400/500/600 л, family Geist (+mono) (02-typography.md)
- [ ] Body line-height 1.5–1.7, гарчиг 1.1–1.3 (02-typography.md)
- [ ] Урт текстийн мөр `max-width: 65ch`-ээс хэтрэхгүй (02-typography.md)
- [ ] **Өө Үү Ёё glyph сонгосон font-д бүрэн бий — `cyrillic-ext` subset ачаалагдсан, fallback руу унаагүй** (09-localization-mn.md)
- [ ] Урт монгол label (товч, tab, table header, nav) overflow хүснэгтийн дагуу шийдэгдсэн (09-localization-mn.md)
- [ ] Font ≤4 woff2, `font-display: swap`, критик 1–2 файл preload (02-typography.md)

## Зай & layout

- [ ] Бүх зай 4/8 scale-ээс (`--spacing-*`); card padding 16/24, 20 биш (03, 06)
- [ ] **320px өргөнд хэвтээ scroll байхгүй** (03-spacing-layout.md)
- [ ] 200% zoom-д контент алдагдахгүй, хэвтээ scroll байхгүй (07-accessibility.md)
- [ ] Pointer target ≥24×24px (touch-first/landing CTA ≥44), хоорондын зай ≥8px (07-accessibility.md)
- [ ] Fixed header/bottom bar `env(safe-area-inset-*)` тооцсон; өндөрт `dvh`, `h-screen` байхгүй (03, 15)
- [ ] Container max-width: dashboard fluid ≤1536, форм/settings/текст 720, landing 1152–1280, prose 65ch (03-spacing-layout.md)
- [ ] Өргөн table/code өөрийн `overflow-x: auto` саванд; grid child `min-width: 0` (03-spacing-layout.md)
- [ ] `html { scrollbar-gutter: stable }`; overlay нээхэд layout үсрэхгүй (14-app-resilience.md)

## Компонент

- [ ] **Нэг view-д primary товч 1 л байна**; бусад нь `variant="secondary"` ил (06-components.md)
- [ ] Товч sm 32 / md 36 / lg 40 / xl 44; table мөр 36/44/52 — scale-ээс гадуур утга байхгүй (00-defaults.md)
- [ ] Label талбарын дээр; placeholder label-ийн оронд биш; `type`/`inputmode`/`autocomplete` матрицын дагуу (06-components.md)
- [ ] Validation: эхний keystroke-д биш, blur-ийн дараа; алдаа талбарын доор, `aria-describedby` + `aria-invalid`; >5 алдаа → error summary (06-components.md)
- [ ] Тоон багана баруун зэрэгцээ + `tabular-nums`; хоосон нүд `—` (06-components.md)
- [ ] Truncate хийсэн текст бүрт `title` эсвэл tooltip бий
- [ ] Icon-only товч бүрт `aria-label` + tooltip; icon хэмжээ текстээ дагасан (04-visual-details.md)
- [ ] Modal: focus trap, Esc хаана, хаагдахад focus trigger рүү буцна; z-index зөвхөн `--z-*` token (06, 08, 15)
- [ ] Destructive үйлдэл: Undo 5с (bulk 10с) эсвэл confirm (үйлдлийн нэртэй товч); буцаашгүй бол type-to-confirm (11-dashboard-patterns.md)
- [ ] Tooltip 500ms delay, chart ≤150ms; dropdown ≤8 item (06, 11)

## Motion

- [ ] Duration зөвхөн 120/160/240ms token (хуудасны шилжилт ≤300); ease-out (05-motion.md)
- [ ] Зөвхөн transform/opacity animate хийсэн — width/height/top/left биш (05-motion.md)
- [ ] **`prefers-reduced-motion: reduce` хүндэтгэсэн** — iteration-count 1, scroll-behavior auto ч орсон (05-motion.md)

## A11y

- [ ] **Зөвхөн Tab-аар хуудсыг бүхэлд нь тойрч, бүх үйлдлийг Enter/Space-ээр хийж чадна** (07-accessibility.md)
- [ ] Heading дараалал h1→h2→h3 алгасаагүй; h1 нэг л байна; `<title>` h1-тэй таарна (07-accessibility.md)
- [ ] `header/nav/main/footer` landmark бүгд бий; skip link ажиллана (07, 15)
- [ ] `<html lang="mn">` / `lang="en"` идэвхтэй хэлээ дагана
- [ ] Зураг бүрт утгатай `alt` (монголоор); чимэглэлийнх `alt=""` эсвэл `aria-hidden`
- [ ] Async үр дүн (хайлт, хадгалсан, алдаа) `aria-live` бүсээр зарлагддаг; бүс DOM-д урьдчилан байна (07-accessibility.md)
- [ ] Drag шаарддаг үйлдэл бүрт drag-гүй хувилбар; paste хориглоогүй (WCAG 2.5.7, 3.3.8)

## Performance

- [ ] LCP ≤2.5s, INP ≤200ms, CLS ≤0.1 (Lighthouse mobile дээр) (13-landing.md)
- [ ] Зураг бүрт `width/height` эсвэл `aspect-ratio`; fold-ийн доорхи `loading="lazy"` (04-visual-details.md)
- [ ] Hero/LCP зураг preload + `fetchpriority="high"`, WebP/AVIF + srcset (04, 12)
- [ ] **Ачаалахад layout shift нүдэнд харагдахгүй — skeleton/aspect-ratio-гоор барьсан**
- [ ] Chart >1,000 цэгт canvas (ECharts) + LTTB; агрегац серверт (12-data-viz.md)

## Локалчлал & бичвэр

- [ ] **Бүх string i18n-ээр; EN/MN хоёуланд түлхүүр бий, fallback түлхүүр нэр харагдахгүй** (09-localization-mn.md)
- [ ] Огноо `yyyy-MM-dd HH:mm`, UTC+8; долоо хоног Даваагаас; timezone нэг газар тогтоосон (09-localization-mn.md)
- [ ] Мөнгө `1,250,000₮` (suffix, зайгүй), `tabular-nums`; утас `+976 XXXX XXXX` (09-localization-mn.md)
- [ ] Хэл: public → URL `/mn/`; апп → сервер + cookie; localStorage-only биш (09-localization-mn.md)
- [ ] Товчны label үйл үг жагсаалтаас; «OK/Тийм/Submit» байхгүй; sentence case (10-ux-writing-mn.md)
- [ ] Динамик нэр дагавартай залгагдаагүй; plural ICU `=0`/`other` (10-ux-writing-mn.md)
- [ ] Алдааны мессеж «юу + яаж засах» — «500 Internal Server Error» биш (10-ux-writing-mn.md)
- [ ] Public copy-д дотоод email, staging URL, IP байхгүй; Lorem ipsum/TODO үлдээгүй

## Тэсвэр (14-app-resilience.md)

- [ ] 404 / 403 / 500 / maintenance хуудас бодит, shell зөв; 403-д «эрх хүсэх»
- [ ] Offline banner, session-expiry modal (2 мин өмнө), 429 countdown, stale «Шинэчлэгдсэн HH:mm»
- [ ] SPA route: `<title>` + фокус h1 + `aria-live` зарлал; chunk-load fail-д нэг reload
- [ ] `Cmd+P` — shell нуугдана, table толгой давтагдана, dark-д цагаан

## Хуудасны төрлөөр нэмэлт gate

### Dashboard хуудас (11-dashboard-patterns.md)

- [ ] `h1` + primary action нэг мөрөнд; гүн ≥3 бол breadcrumb; sidebar active bar + weight
- [ ] Шүүлтүүр/сорт/хуудас/tab/`q` URL-д; density/sidebar localStorage-д; refresh, back, share ажиллана
- [ ] Table: sort 3 шат, filter chip + тоо, bulk bar, row action ≤2 + `⋯`, mobile эхний багана sticky
- [ ] Pagination 25/50/100, «1–25 / 1,240»; >10k мөрт cursor; infinite scroll зөвхөн feed
- [ ] Unsaved-changes guard; settings хэсэг ≤720px; autosave ба «Хадгалах» хольсонгүй
- [ ] Tenant нэр + орчны баннер ил; `Esc`/`/`/`Cmd+K` ажиллана; ≤1024 drawer

### Chart / data-viz (12-data-viz.md)

- [ ] Chart бүр нэг асуултад хариулна; pie ≤5 зүсэм; 3D/dual axis/radar/gauge байхгүй
- [ ] Bar 0-оос; tick ≤6; тоо товчилсон (`1.2K`), ₮ suffix; нэгж тэнхлэгийн гарчигт
- [ ] ≤6 categorical `--chart-1…6`, нэг entity нэг өнгө; өнгөнөөс гадна label/pattern/marker
- [ ] KPI tile: label 12 / value 28 / delta сум+тэмдэг+өнгө / харьцуулсан хугацаа
- [ ] `<figure>` + `aria-label` + хүснэгт хувилбар; tooltip keyboard-оор; mark contrast ≥3:1
- [ ] 5 төлөв; өгөгдөлгүй = null завсар; partial hatched; өндөр ≥160px, container query

### Landing (13-landing.md)

- [ ] h1 нэг, ≤10 үг, үр ашиг; title ≤60, description ≤155; OG 1200×630
- [ ] Primary CTA нэг label, 3–4 удаа; hero-д ≤2 товч; xl 44px
- [ ] Hero бүтэн дэлгэц биш (560–720); autoplay видео/carousel байхгүй; reveal 240ms нэг удаа
- [ ] Container 1152–1280; section зай 64–96 / 48–64; h2 нэг/хэсэг
- [ ] JS ≤150KB gz; font ≤4 woff2; third-party зөвхөн analytics, defer, consent-ийн дараа
- [ ] canonical, sitemap, `hreflang` mn/en/x-default, JSON-LD validator-т алдаагүй
- [ ] Privacy/Terms/холбоо барих бодит; public email л; © он зөв
- [ ] 320 / 375 / 768 / 1280 / 1920-д overflow, давхцал, зураг тасралт байхгүй

## Автоматжуулалт (CI-д)

| Юу | Хэрэгсэл | Босго |
|---|---|---|
| A11y дүрэм | Playwright + `@axe-core/playwright` — хуудас бүрийн 5 төлөв, light + dark, 320/768/1280 | violations = 0 (`serious`, `critical`) |
| Keyboard | Playwright: `Tab` цикл → focus-visible snapshot; `Esc` overlay хаана | Фокус алдагдсан элемент 0 |
| Core Web Vitals | Lighthouse CI (`lhci autorun`, mobile preset), budget.json | perf ≥90, a11y = 100, LCP ≤2.5s, CLS ≤0.1, JS ≤150KB |
| Contrast lint (token) | Script: theme.css-ийн semantic хос бүрийг (`foreground-*` × `background-*`, `on-*` × `*-solid`, `border-input` × `background`) WCAG ratio + APCA (`apca-w3`) тооцож тайлан | текст ≥4.5, UI ≥3; APCA body ≥Lc 75 (анхааруулга) |
| Screenshot diff | Playwright `toHaveScreenshot` — компонент бүрийн 5 төлөв × 2 theme × 3 viewport; `maxDiffPixelRatio: 0.01` | Зөрөө = ухамсартай review |
| i18n | Test: `mn`/`en` түлхүүр олонлог тэнцүү; JSX-д кирилл/латин literal string lint (`eslint-plugin-i18next` `no-literal-string`) | 0 |
| Token хэрэглээ | Stylelint: `color-no-hex`, `declaration-property-value-disallowed-list` (`z-index: /\d/`, `/\d+vh/` — `dvh` биш vh) | 0 |
| Шрифт | Build: woff2 файлын тоо ≤4, нийт ≤100KB; `cyrillic-ext` байгаа эсэх | fail on exceed |

Хурдан гараар шалгах хэвээр үлдэнэ — автомат нь «байхгүй»-г л барина, «муу»-г барихгүй.

## Хэрхэн ажиллуулах

1. Browser-ийг **320 / 768 / 1280** өргөнд (landing-д + 375 / 1920) нээж хуудас бүрийг гүйлгэнэ.
2. 200% zoom (Cmd/Ctrl +) — хэвтээ scroll, давхцал хайна.
3. Хулганагүй: зөвхөн Tab/Shift+Tab/Enter/Esc-ээр бүх flow-г дуустал явна.
4. Dark mode асааж 1–3-ыг давтана.
5. Lighthouse (mobile) + axe DevTools — алдаа 0, performance ≥90.
6. Desktop + mobile screenshot аваад хажуу хажууд нь харж эцсийн нүдний шалгалт (320/375-д Playwright headless — Chrome ≥500px clamp хийдэг).
7. Хуудасны төрлийн gate (dashboard / chart / landing) + тэсвэрийн хэсгийг давна.

## Эх сурвалж

- WCAG 2.2 — SC 1.4.3, 1.4.4, 1.4.10, 1.4.11, 2.1.1, 2.4.7, 2.5.7, 2.5.8, 3.1.1, 3.3.8, 4.1.3 — https://www.w3.org/TR/WCAG22/
- web.dev — "Web Vitals"; "Optimize Cumulative Layout Shift"; "Lighthouse CI" — https://web.dev
- Deque — axe-core, `@axe-core/playwright` — https://github.com/dequelabs/axe-core-npm
- Playwright — Visual comparisons (`toHaveScreenshot`), Accessibility testing — https://playwright.dev/docs
- Google — Lighthouse CI, performance budgets (`budget.json`) — https://github.com/GoogleChrome/lighthouse-ci
- APCA — `apca-w3` — https://github.com/Myndex/apca-w3
- Stylelint — `color-no-hex`, `declaration-property-value-disallowed-list`; eslint-plugin-i18next — `no-literal-string`
- MDN — `prefers-reduced-motion`, `env()` (safe-area-inset), `font-display`, `scrollbar-gutter`, ARIA live regions — https://developer.mozilla.org
- Nielsen Norman Group — "Designing Empty States", "Error-Message Guidelines" — https://www.nngroup.com
- Apple HIG — "Layout" (safe area); Material 3 — "Accessibility", "States" — https://m3.material.io
