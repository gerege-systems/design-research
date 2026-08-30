[← Индекс руу буцах](README.md)

# Апп-ын тэсвэр — алдааны хуудас, offline, session, давхарга, хэвлэх, SPA

## Яагаад

«Happy path» бэлэн болсны дараа апп хамгийн ихээр эвдэрдэг газрууд: 404/403, сүлжээ тасрах, session дуусах, rate limit, хуучирсан өгөгдөл, z-index дайн, scroll lock-оос layout үсрэх, хэвлэхэд sidebar гарч ирэх, SPA-д route солиход screen reader чимээгүй. Энэ файл тэр бүгдэд **нэг л шийдэл** өгнө. Компонентын 5 төлөв (06) ба dashboard-ын урсгал (10)-ын үргэлжлэл.

## Алдааны хуудсууд

| Код | Гарчиг (h1) | Body | Үйлдэл |
|---|---|---|---|
| 404 | «Хуудас олдсонгүй» | URL буруу эсвэл хуудас зөөгдсөн; юу бичснийг давтахгүй | [Нүүр хуудас] + хайлт (апп-д); landing-д алдартай 3 линк |
| 403 | «Танд энэ хуудсыг үзэх эрх алга» | Хэн эрх өгч болох (admin, багийн эзэн) | [Эрх хүсэх] (primary) + [Буцах]; нэвтрээгүй бол 403 биш login руу redirect + `?next=` |
| 500 | «Алдаа гарлаа» | «Бид мэдээллийг авлаа»; request ID жижиг muted (`req_8f3a…`) — support-д хэлэх; stack/техник текст хэзээ ч биш | [Дахин оролдох] + [Нүүр] |
| Maintenance (503) | «Түр засвартай» | Хэзээ дуусах (`yyyy-MM-dd HH:mm` хүртэл), статус хуудасны линк | Автомат retry 60с; `Retry-After` header |

- Алдааны хуудас app shell-тэй (sidebar үлдэнэ) — 403/404 дотроос; 500/503 нь shell-гүй, статик (апп өөрөө унасан байж болно).
- `<title>`-д код + гарчиг: «404 — Хуудас олдсонгүй»; `h1` фокус авна (доор SPA).
- Illustration ≤200px, сонголт; emoji биш.

## Сүлжээ, session, хязгаар

- **Offline banner**: `navigator.onLine` + fetch алдаа хосолсон; дээд талд 32–40px, `--warning-subtle` фон, `role="status"`: «Интернэт холболт алга — өөрчлөлт хадгалагдахгүй». Холбогдоход 2с «Холбогдлоо» → алга. Offline үед submit товч disabled биш — дарахад queue/алдаа тайлбар.
- **Session дуусах**: дуусахаас **2 минутын өмнө** modal (`role="alertdialog"`): «Идэвхгүй байсан тул 2:00 минутын дараа гарна» · [Үргэлжлүүлэх] (primary, session сунгана) [Гарах]. Countdown `aria-live="polite"` 30с тутам. Дуусвал login руу `?next=` + формын draft localStorage-д (06 → Autosave) — дахин нэвтрэхэд сэргэнэ. Token refresh чимээгүй явагдах бол modal хэрэггүй — зөвхөн refresh боломжгүй үед.
- **Rate limit (429)**: inline, тухайн үйлдлийн дэргэд: «Хэт олон оролдлого. 45 секундын дараа дахин оролдоно уу» — `Retry-After`-оос countdown, товч тэр хүртэл disabled. Login дээр аль талбар буруу гэдгийг хэлэхгүй хэвээр.
- **Stale data**: амьд өгөгдөл (dashboard KPI, жагсаалт) сүүлд шинэчлэгдсэн цагаа харуулна («Шинэчлэгдсэн 14:32»); >5 минут хуучирвал muted → warning өнгө + [Шинэчлэх]. Tab идэвхгүй болоод буцаж ирэхэд (`visibilitychange`) refetch. Засах формд: хадгалах үед серверийн хувилбар өөрчлөгдсөн бол (ETag/`updated_at`) 409 → «Өөр хүн зассан байна» · [Тэдний хувилбарыг үзэх] [Дарж хадгалах].
- **Retry**: автомат retry зөвхөн GET-д (3 удаа, exponential 1/2/4с); POST/PUT-д хэзээ ч автоматаар биш — хэрэглэгч [Дахин оролдох] дарна; idempotency key байвал автомат зөвшөөрнө.

## Давхарга (layering)

- Z-index зөвхөн token (08): dropdown 1000 < sticky 1100 < overlay 1200 < modal 1300 < popover 1400 < toast 1500 < tooltip 1600. Popover modal-аас дээр (modal доторх select/date picker ажиллахын тулд); toast modal-аас дээр (хадгалсан мэдэгдэл харагдана); tooltip хамгийн дээр.
- **Top layer**: `<dialog>.showModal()` ба `popover` attribute-тай элемент z-index-ээс үл хамааран хамгийн дээр гардаг — хоёр top-layer элемент нээгдсэн дарааллаараа давхарлана. Custom z-index-тэй хуучин modal + шинэ `<dialog>` хольбол дараалал таамаглашгүй → нэг апп-д нэг механизм.
- Stacking context-ийг санамсаргүй үүсгэхгүй: `transform`, `filter`, `opacity<1`, `will-change` бүхий эцэг доторх `position: fixed` элемент тэр эцгээр хязгаарлагдана. Overlay-уудыг `<body>`-ийн шууд хүүхэд (portal) болго.
- **Scroll lock**: modal/drawer нээхэд `html { overflow: hidden }` + scrollbar-ийн өргөнөөр layout үсрэхээс `html { scrollbar-gutter: stable }` (бүх хуудсанд, зөвхөн modal үед биш). iOS-д `overflow: hidden` хангалтгүй — `position: fixed; top: -{scrollY}px; width: 100%` + хаахад scroll сэргээх. Нэг scroll lock counter (олон overlay зэрэг нээгдэхэд сүүлийнх нь хаагдахад л тайлна).
- Overlay-ийн гадна дарахад хаагдах, Esc, focus trap, `inert` — 06 Modal, 07.

## Хэвлэх (print)

```css
@media print {
  nav, aside, .sidebar, .topbar, .toast, .no-print, [data-print="hide"] { display: none !important; }
  main { max-width: none; padding: 0; }
  a[href^="http"]::after { content: " (" attr(href) ")"; font-size: 0.8em; }
  table { break-inside: auto; } tr, img, figure { break-inside: avoid; }
  thead { display: table-header-group; }
  * { box-shadow: none !important; color-scheme: light; }
  @page { margin: 16mm; }
}
```

- Хэвлэх ёстой хуудас (нэхэмжлэх, тайлан, баримт) `.print-only` header (лого, огноо, хуудасны дугаар `counter(page)`)-тэй; бусад хуудсанд print stylesheet зөвхөн shell нуух.
- Dark theme-д хэвлэвэл цагаан дэвсгэр: `color-scheme: light` + `.dark` token-уудыг `@media print`-д light утгаар дарах, эсвэл хэвлэхээс өмнө class хасах.
- Chart хэвлэхэд canvas бүдгэрдэг — SVG эсвэл `<table>` хувилбар (11).

## SPA route солигдоход

1. `document.title` = «Хуудасны нэр — Апп» (h1-тэй ижил).
2. Фокусыг **`h1`** (`tabindex="-1"`) эсвэл `<main>` руу; scroll дээш (`history.back`-д байрлал сэргээх — router-ийн scroll restoration).
3. `aria-live="polite"` бүсэд «{Хуудасны нэр} хуудас ачаалагдлаа» — бүс app shell-д байнга байна.
4. Skip link («Гол контент руу») shell-ийн эхний элемент; route бүрт `<main id="main">`.
5. Route-level loading: 300ms-ээс хурдан бол юу ч биш; удаан бол дээд талд 2px progress bar (`--accent`) + хуучин хуудас үлдэнэ, бүтэн skeleton биш.
6. Route алдаа (chunk load fail — deploy дараах хуучин hash): нэг удаа автоматаар `location.reload()`, давтвал «Шинэ хувилбар гарсан — [Шинэчлэх]».

## Mobile shell — bottom tab bar, PWA

- ≤768px-д sidebar-ийн оронд **bottom tab bar**: 3–5 tab, өндөр 56px + `env(safe-area-inset-bottom)`, icon 24 + label 12px (label заавал), active = accent icon + label (өнгө + weight). 6 дахь зүйл «Цэс» tab → drawer. `--z-sticky`.
- Primary action (шинээр үүсгэх) bottom bar-ын дунд эсвэл FAB (56px, баруун доод, safe-area) — нэг л.
- Bottom bar байхад хуудасны доод padding = bar өндөр; keyboard гарахад bar нуугдана (`visualViewport` resize).
- **PWA суурь**: `manifest.webmanifest` (`name`, `short_name` ≤12 тэмдэгт, `start_url`, `display: standalone`, `theme_color` = `--background` (light), `background_color`, icon 192/512 + `maskable`); `<meta name="theme-color">` light/dark хоёр (`media="(prefers-color-scheme: dark)"`); service worker зөвхөн shell + font cache (offline-first өгөгдөл — тусдаа шийдвэр); install prompt-ыг хэрэглэгч 2+ удаа орсны дараа, автоматаар биш.
- Standalone горимд browser back байхгүй — хуудас бүрт «← Буцах» (гүн ≥2) заавал.
- `viewport-fit=cover`, `100dvh`, `overscroll-behavior: none` (pull-to-refresh-ийг өөрийн refresh-тэй зөрчилдүүлэхгүй).

## Шалгах

1. `/abc`, эрхгүй `/admin`, серверийг унтраагаад, offline болгоод — 4 дэлгэц зөв, shell зөв эсэх.
2. Session дуусахыг 3 минут болгоод modal → сунгах → дуусах → login → draft сэргэх.
3. Modal + доторх select + toast зэрэг нээгдэхэд дараалал зөв, scroll lock-оос layout үсрэхгүй (scrollbar-gutter).
4. `Cmd+P` — sidebar алга, table толгой хуудас бүрт, dark-д цагаан.
5. Route солиход VoiceOver гарчгийг уншиж, фокус h1 дээр.
6. 375px-д bottom bar safe-area, keyboard гарахад нуугдана; Lighthouse PWA installable.

## Эх сурвалж

- WCAG 2.2 — 2.4.3 Focus Order, 2.2.1 Timing Adjustable, 2.2.6 Timeouts (AAA), 4.1.3 Status Messages, 2.4.1 Bypass Blocks
- WAI-ARIA APG — Alert Dialog pattern; Live regions
- MDN — `<dialog>`, Popover API, top layer; `scrollbar-gutter`; `overscroll-behavior`; `visibilitychange`; `navigator.onLine`; `@media print`, `break-inside`; Web app manifest; `theme-color`
- web.dev — "Building a dialog component"; "Learn PWA" (manifest, installability); "Offline UX design guidelines"; "Handle 'Retry-After'"
- Nielsen Norman Group — "Error-Message Guidelines"; "404 Pages"; "Timeout warnings"; "Mobile navigation: bottom tab bars"
- Apple HIG — Tab bars; Material 3 — Navigation bar (bottom)
- IETF RFC 9110 — 429 Too Many Requests, `Retry-After`; RFC 7232 — ETag / conditional requests
- Marcy Sutton — "Accessible client-side routing" (focus + announce)
