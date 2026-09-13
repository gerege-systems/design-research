[← Индекс руу буцах](README.md)

# Default-ууд — бодолгүй хэрэглэх нэг утга

## Яагаад

Бусад файлд муж («36–44px») бий; энд муж бүрээс **нэг л default**. Агент, хүн хоёулаа эргэлзэхгүй эндээс авна; өөр утга сонговол шалтгаанаа бичнэ. Утгууд `gerege-ui` theme.css-тэй ижил; зөрвөл theme.css + энэ файл зөв, бусад файл засагдана.

## Хүснэгт

| Сэдэв | Default | Хэзээ өөр | Эзэн файл |
|---|---|---|---|
| Pointer target | 24×24px (хөрш хооронд ≥8px) | Touch-first дэлгэц, landing CTA → 44×44 | 07 |
| Товчны өндөр | md 36px | sm 32 (compact dashboard), lg 40, xl 44 (marketing/touch) | 06 |
| Товчны variant | `secondary`-г ил бич; view-д `primary` 1 | — | 06 |
| Input өндөр | 36px; mobile font 16px | desktop-д 14px болно | 06 |
| Table мөр | 44px | compact 36, comfortable 52 | 06 / 10 |
| Table header | 12–13px, 500, sentence case | uppercase → tracking 0.06em | 06 |
| Card padding | 24px desktop / 16px mobile | хэзээ ч 20 биш | 06 |
| Font | Geist (cyrillic-ext), weight 400/500/600 | Inter зөвхөн fallback | 02 / 09 |
| Font файл | ≤4 woff2, ≤100KB критик зам | — | 02 |
| Type scale | апп 1.2 @ 14px · маркетинг 1.25 @ 16px; ≤8 хэмжээ | — | 02 / 08 |
| Min font | 12px | chart tick 11px | 02 / 11 |
| Body line-height | 1.5 (апп 14/24) | гарчиг 1.1–1.3 | 02 |
| Uppercase tracking | хэрэглэхгүй (sentence case) | шаардвал 0.06em (кирилл) | 09 |
| Spacing | 4/8 scale; section зай 64px desktop / 48 mobile | — | 03 / 12 |
| Max-width | dashboard fluid ≤1536 · форм/settings/текст 720 · landing 1280 · prose 65ch | — | 03 |
| Sidebar | 240px дэлгэсэн / 56 хумьсан (rail); ≤1024 drawer | — | 10 |
| Breakpoints | 640 / 768 / 1024 / 1280 / 1536 | контентоор нэм | 03 |
| Radius | товч/input 6 · card 8 · modal 12 · pill full | sm 4 badge | 04 |
| Shadow | байхгүй (border 1px) | хөвөгч: dropdown md, modal/drawer lg; xl зөвхөн том marketing overlay | 04 |
| Border өнгө | `--border`; interactive control → `--border-input` (зөөлөн ~1.5:1; prefers-contrast дээр ≥3:1) | — | 04 / 08 |
| Icon | lucide, 16px @ 14px текст, stroke 2 | 20px @ 16–18px текст, 24 гарчиг | 04 |
| Accent | 1 hue, C ≤0.2; 60-30-10; accent-fill view-д 1 | — | 01 |
| Contrast | текст 4.5:1; UI/border/icon 3:1 бүх фон дээр | — | 07 |
| Focus ring | `2px solid var(--ring)`, offset 2px | accent-fill дээр давхар ring | 07 |
| Motion | 160ms ease-out; hover 120; modal 240; page ≤300 | reduced-motion → унтраа | 05 |
| Tooltip delay | 500ms | chart ≤150ms | 06 / 11 |
| Skeleton | 300ms хойшлуул, ≥500ms үлдээ | — | 10 |
| Toast | success 4с; error гараар хаана; Undo 5с | bulk destructive Undo 10с | 06 / 10 |
| Destructive | Undo toast (буцаагдах) · modal confirm · type-to-confirm (буцаашгүй) | — | 10 |
| Validation | blur дараа; алдааны дараа input бүрт; >5 алдаа → error summary | — | 06 |
| Date | `yyyy-MM-dd HH:mm`, UTC+8, Даваа эхэлнэ | ≤7 хоног relative + title | 09 |
| Currency | `25,000₮` (suffix, зайгүй); `12.4M₮` | — | 09 |
| Утас | `+976 XXXX XXXX`; хадгал E.164 | — | 09 |
| Number | `1,250,000.50` | — | 09 |
| Хэл | public → URL `/mn/`; апп → server setting + cookie | localStorage-only хэзээ ч биш | 09 |
| Prefs хадгалах | density/sidebar → localStorage · filter/page/sort → URL · account → server | — | 10 |
| Theme | `.dark` class, pre-paint script, 3 төлөв | `data-theme` биш | 08 |
| Z-index | token л (`--z-*` 1000…1600) | — | 08 / 15 |
| Viewport өндөр | `dvh` | `svh` header/hero | 03 |
| Pagination | offset, 25/хуудас, URL `?page=&size=` | >10k мөр → cursor | 10 |
| Search | debounce 300ms, ≥2 тэмдэгт, URL `?q=` | — | 10 |
| Chart сан | Энгийн line/bar/area (≤6 series, ≤1,000 цэг): `@gerege-systems/ui` Chart (SVG, token, table fallback) эсвэл Recharts | Нарийн chart → Recharts; >1,000 цэг → ECharts | 11 |
| Chart өнгө | `--chart-1…6`, accent биш; ≤6 series | — | 11 |
| KPI мөр | 4 tile, label 12 / value 28 / delta 12 | — | 11 |
| Modal өргөн | форм 520px | alert 400, том 720 | 06 |
| Landing CTA | 1 label, 3–4 удаа; xl товч | — | 12 |
| Landing budget | LCP 2.5s · INP 200ms · CLS 0.1 · JS ≤150KB gz | — | 12 |
| Test viewport | 320 / 768 / 1280 | landing + 375 / 1920 | 13 |
| Component states | loading / empty(first-run\|filtered) / error / success / permission-denied | — | 06 |
| Alt text / aria-label | Монголоор, i18n-ээр | — | 16 |

## Хэрэглэх

1. Шинэ компонент/хуудас эхлэхдээ энэ хүснэгтээс утгаа аваад шууд бич.
2. Default-аас хазайвал кодын комментод `// design: 52px row — dense finance table` маягаар шалтгаан.
3. 15-checklist.md-ийн шалгалт энэ хүснэгтийг ишилдэг — хоёр газар өөр утга гарвал энд засна, тэнд биш.

## Эх сурвалж

Эндэх утга бүрийн эх сурвалж эзэн файлд нь бий (хүснэгтийн сүүлийн багана); нэмэлт эх энд давтахгүй.
