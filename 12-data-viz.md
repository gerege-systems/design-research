[← Индекс руу буцах](README.md)

# Өгөгдлийн дүрслэл (Dashboard)

## Яагаад

Dashboard-ын chart нь чимэглэл биш — асуултад хариулах хэрэгсэл. Буруу төрөл, илүүдэл өнгө, 3D, давхар тэнхлэг нь уншихад саад болж, хэрэглэгчийг буруу дүгнэлтэд хүргэдэг. **Chart бүр нэг асуултад хариулна; хариулт 5 секундэд уншигдахгүй бол chart буруу.** Data-ink ratio: зураасны дийлэнх нь өгөгдөл байг, хүрээ/дэвсгэр/сүүдэр биш.

## Chart төрөл сонгох

| Асуулт | Төрөл | Санамж |
|---|---|---|
| Харьцуулалт (ангилал хооронд) | Bar (хэвтээ — урт label-тай бол) | Үргэлж 0-оос эхэл |
| Хандлага (цаг хугацаанд) | Line | ≤4 шугам; илүү бол small multiples |
| Бүхлийн хэсэг | Stacked bar / 100% bar | Хугацааны дагуу харьцуулахад |
| Бүхлийн хэсэг, нэг агшин | Pie — **зөвхөн ≤5 зүсэм** | Бүхлийн хэсэг биш бол pie огт биш |
| Тархалт | Histogram / box plot | Bin-ийн тоо 10-20 |
| Хамаарал | Scatter | Трэнд шугам нэмж болно |
| Ганц тоо | KPI tile | Доор үз |
| Жижиг хандлага | Sparkline | Тэнхлэггүй, 100-160px өргөн |

Хориглоно: **3D ямар ч хэлбэрээр**; хоёр y-тэнхлэг (dual axis) — хүчтэй шалтгаангүй бол (хоёр chart давхарлах нь дээр); donut/pie >5 зүсэмтэй; radar/gauge — чимэглэл л болдог.

## KPI tile-ийн анатоми

```
Нийт орлого              ← label 12-13px, muted
12.4M₮                   ← value 24-32px, 600 weight, tabular-nums
▲ +8.2%  өмнөх сартай    ← delta: сум + өнгө + тэмдэг; харьцуулсан хугацаа 12px muted
▁▂▃▅▆█                   ← sparkline (заавал биш), 24-32px өндөр
```

- Value-д `font-variant-numeric: tabular-nums` — тоо солигдоход өргөн үсрэхгүй.
- Delta-д **өнгө ганцаараа хүрэлцэхгүй**: сум (▲/▼) + тэмдэг (+/−) + өнгө гурвуулаа.
- Харьцуулсан хугацааг үргэлж бич («өмнөх 30 хоног», «өнгөрсөн сар») — хугацаагүй delta утгагүй.
- Нэг мөрөнд **4-6 tile**; mobile-д 2 багана. Tile-ийн padding 16-24px (06-components.md-ийн card-ыг үз).
- Tile бүр дээр дарахад дэлгэрэнгүй рүү орох бол бүхэлдээ clickable, hover төлөвтэй.

## Өнгө

- **Categorical** (ангилал): **≤6 hue**, lightness нь ялгаатай, colorblind-safe палитр (Okabe-Ito 8 өнгө, Tableau 10 маягийн). Нэг entity = бүх chart дээр нэг өнгө (жишээ: «Улаанбаатар» хаа сайгүй нэг цэнхэр). Брэндийн accent-ийг **series 1 болгохгүй** — highlight/сонгосон элементэд хадгал.
- **Sequential** (хэмжигдэхүүн): нэг hue-ийн lightness ramp, **5-7 шат**. Heatmap, choropleth-д. Rainbow (jet) хориотой.
- **Diverging** (төвтэй): хоёр hue, дундад саарал/цайвар төв (жишээ: цэнхэр ← саарал → улбар шар). Төв нь утга учиртай цэг (0, дундаж, зорилт) байх ёстой.
- **Semantic**: «өсөлт = ногоон» нь үргэлж үнэн биш — зардлын өсөлт муу. Санхүүд ашиг ногоон / алдагдал улаан нь конвенц, гэхдээ **үргэлж тэмдэг эсвэл icon-той хослуул**. Улаан-ногоон хослол protanopia/deuteranopia-д ялгагддаггүй → улаан-цэнхэр, улбар шар-цэнхэр давуу.
- **Dark mode**: series өнгийг бага зэрэг desaturate + lighten (chroma −10-20%, lightness +10-15%); light mode-ын тод өнгө хар дэвсгэр дээр «гэрэлтдэг». Gridline dark-д бүр бүдэг (01-color.md-г үз).
- **Өнгөнөөс хамаарахгүй**: шууд label, pattern (зураас/цэг), эсвэл marker хэлбэр (○ □ △). Шугаман chart-д хэлбэр + dash давхар ялгаа.

## Тэнхлэг ба gridline

- **Bar chart-ын утгын тэнхлэг заавал 0-оос.** Line chart 0-оос эхлэхгүй байж болно — гэхдээ тэнхлэгийн утгыг ил харуул.
- Tick: **≤6** тэнхлэг бүрт; «сайхан» тоо (0, 5, 10… эсвэл 0, 25, 50…).
- Товчлол: `1.2K`, `3.4M`, `1.1B` — tick бүрт бүтэн тоо бичихгүй. Нэгжийг tick бүрт биш тэнхлэгийн гарчигт («Орлого, ₮ сая»).
- Gridline: зөвхөн хэвтээ (bar/line), 1px, border-subtle өнгө; босоо gridline ихэвчлэн хэрэггүй.
- Chart junk-гүй: chart-ийн хүрээ (border) байхгүй, plot area-д дэвсгэр өнгө байхгүй, тэнхлэгийн шугам 1px эсвэл огт байхгүй, сүүдэр/gradient байхгүй.
- Тэнхлэгийн label 12px muted (tick-д 11px зөвшөөрнө — UI-ийн 12px доод хязгаарын цорын ганц үл хамаарах); налуу (rotated) label-аас зайлсхий — хэвтээ bar эсвэл цөөн tick.

## Label ба legend

- **≤4 series бол legend биш шууд label** — шугамын төгсгөлд нэр, bar-ын дээр утга.
- Legend хэрэгтэй бол байрлал: дээр (хэвтээ) эсвэл баруун (босоо); доор нь зөвхөн mobile-д. Дараалал нь chart дээрх дарааллаар (хамгийн дээд шугам = legend-ийн эхнийх).
- Legend-ийн зүйл дээр дарахад series toggle хийдэг байвал сайн.
- Гарчиг = дүгнэлт биш асуулт/хэмжигдэхүүн («Сарын орлого, 2026») 14px 600; subtitle 12px muted (хугацаа, шүүлтүүр).

## Tooltip

- Hover **ба** focus дээр гарна (keyboard-оор хүрнэ); 150ms-аас удаан delay байхгүй.
- Агуулга: огноо/ангилал бүтэн, бүх series-ийн бүтэн утга (товчлолгүй), нэгжтэй. Олон series-д series-ийн өнгийн цэг + нэр + утга нэг мөрөнд.
- Line chart-д нэг x-утгын бүх цэгийг нэг tooltip-д (crosshair), тус тусад нь биш.
- Дэлгэцийн захад автоматаар эргэнэ; курсорыг халхлахгүй (8px offset).

## Тоо, мөнгө, хугацаа

- Тооны формат 09-localization-mn.md-г үз: мянгатын тусгаарлагч, ₮ тэмдэг.
- Хувь **1 орон** (`8.2%`); мөнгө tile-д товчилсон (`12.4M₮`), tooltip-д бүтэн (`12,400,000₮`) — ₮ үргэлж ард, зайгүй.
- Хугацааны тэнхлэг: **нэг chart дотор нэг granularity** (өдөр ЭСВЭЛ 7 хоног ЭСВЭЛ сар); timezone **UTC+8** (Asia/Ulaanbaatar) — серверийн UTC-г шууд зурахгүй.
- **Өгөгдөлгүй хугацаа = завсар**, 0 биш (`null` → line тасарна). 0 нь бодит хэмжилт.
- Огнооны tick: эхний tick-д он/сар бүтэн, дараагийнхад зөвхөн өдөр («Мар 1, 2, 3…»).

## Responsive

- Өндөр viewport-ын дагуу багасна, **доод хязгаар 160px** (sparkline 24-40px).
- <640px: legend доош шилжинэ, tick 3-4 болно, y-тэнхлэгийн label chart дотор (inline) болж болно.
- Өргөн bar chart mobile-д хэвтээ bar болгох нь хэвтээ scroll-оос дээр.
- Container query (`@container`) эсвэл ResizeObserver — viewport биш контейнерийн өргөнөөр.

## Төлөвүүд

- **Loading**: chart-ийн хэлбэртэй skeleton (тэнхлэг + бүдэг bar/шугам), контент ирэхэд өндөр өөрчлөгдөхгүй.
- **Empty**: «Энэ хугацаанд өгөгдөл алга» + хугацааг өөрчлөх/шүүлтүүр арилгах үйлдэл. Хоосон тэнхлэг зурж орхихгүй.
- **Error**: «Өгөгдөл ачаалж чадсангүй» + Дахин оролдох товч (06-components.md-г үз).
- Өгөгдөл дутуу (partial): бүрэн бус хугацааг hatched/бүдэг + тайлбар («Өнөөдөр бүрэн бус»).

## Accessibility

- `<figure>` дотор chart, `<figcaption>`-д гарчиг + нэг өгүүлбэр дүгнэлт («Орлого 3-р сард 12% өсөв»).
- Chart-ийн root-д `role="img"` + `aria-label` (товч дүгнэлт). Decorative sparkline-д `aria-hidden="true"`, утгыг нь текстээр хажууд нь.
- **Өгөгдлийн хүснэгт хувилбар** — «Хүснэгтээр харах» toggle эсвэл visually-hidden `<table>`; screen reader-т chart биш хүснэгт л уншигдана.
- Mark-ийн contrast дэвсгэртэй **≥3:1** (WCAG 1.4.11), зэргэлдээ series хоорондоо ≥3:1 эсвэл pattern/border-оор тусгаарла.
- Tooltip-ыг keyboard-оор (Tab/сум) хүрдэг, `prefers-reduced-motion`-д chart-ийн enter animation унтарна (05-motion.md-г үз).
- Текст ≥12px (tick label 11px зөвшөөрнө), тэнхлэгийн label ≥4.5:1 (07-accessibility.md-г үз).

## Performance

- **SVG ≤~1,000 цэг**; үүнээс дээш canvas (эсвэл WebGL). 5,000+ цэгтэй line-ыг SVG-ээр зурвал scroll/hover гацна.
- Downsample: **LTTB** (Largest-Triangle-Three-Buckets) — хэлбэр хадгалан цэгийг дэлгэцийн пикселийн тоонд ойртуулна (1 px ≈ 1 цэг).
- Агрегацыг серверт (SQL) хий; клиент рүү түүхий мөр биш bucket илгээ.
- Chart component-ыг lazy load (`dynamic import`), tooltip-д debounce 16ms (нэг frame).

## Сан (library)

Нэг проектод **нэг л chart сан**. Энгийн line/bar/area-д `@craftzbay/ui`-ийн Chart (SVG, token-оор өнгө, table fallback, 5 төлөв) хангалттай; түүнээс нарийн (stacked, scatter, brush, tooltip-лог) бол **Recharts** (React, SVG) — chart бүрт ≤1,000 цэг үед; түүнээс дээш (лог, tick data, heatmap) **ECharts** (canvas). d3 зөвхөн custom дүрслэлд, Chart.js-ийг шинэ проектод сонгохгүй. Сангийн default theme-ийг бүү хэрэглэ — өнгө (`--chart-1…6`), font, gridline-ыг semantic token-оос (08-design-tokens.md-г үз).

## Annotation

- **Зорилтын шугам** (goal/target): dashed 1px, muted өнгө, төгсгөлд label («Зорилт 10M₮»). Chart бүрт ≤1.
- **Үйл явдлын тэмдэг** (release, кампанит ажил): босоо 1px шугам эсвэл тэнхлэг дээрх marker + tooltip; ≤3 нэг chart-д.
- Threshold бүс (доод/дээд хязгаар): 5-10% opacity-тай fill.
- Annotation өгөгдлийн өнгөтэй давхцахгүй — саарал/foreground-muted.

## Шалгах жагсаалт

1. Chart бүр нэг асуултад хариулж байна уу; pie ≤5 зүсэм, 3D/dual axis байхгүй юу?
2. Bar chart 0-оос эхэлж байна уу; tick ≤6, тоо товчилсон уу?
3. Categorical ≤6 өнгө, colorblind-safe, нэг entity бүх chart-д нэг өнгө үү?
4. Өнгөнөөс гадна label/pattern/сум/тэмдэг байна уу?
5. KPI tile: label-value-delta-хугацаа дөрвүүлээ, tabular-nums уу?
6. Loading / empty / error / success / permission-denied — 5 төлөвийн загвар (06-components.md) хэрэгжсэн үү; завсар 0 биш null уу?
7. `<figure>` + aria-label + хүснэгт хувилбар, mark contrast ≥3:1 үү?
8. >1,000 цэгт canvas + LTTB; нэг проектод нэг сан уу?

## Эх сурвалж

- WCAG 2.2 — SC 1.4.1 Use of Color, SC 1.4.3 Contrast (Minimum), SC 1.4.11 Non-text Contrast, SC 1.1.1 Non-text Content — https://www.w3.org/TR/WCAG22/
- Edward Tufte — *The Visual Display of Quantitative Information* (data-ink ratio, chart junk)
- Stephen Few — *Information Dashboard Design*; *Show Me the Numbers*
- Okabe & Ito — Color Universal Design palette — https://jfly.uni-koeln.de/color/
- Tableau — «Tableau 10» palette (Maureen Stone, "How We Designed the New Color Palettes in Tableau 10")
- Nielsen Norman Group — "Dashboards: Making Charts and Graphs Easier to Understand"; "Data Tables: Four Major User Tasks"
- Material Design 3 — Data visualization guidance (m3.material.io)
- Apple HIG — Charts — https://developer.apple.com/design/human-interface-guidelines/charts
- MDN — `<figure>` and `<figcaption>`; ARIA `img` role; `font-variant-numeric`
- Sveinn Steinarsson — "Downsampling Time Series for Visual Representation" (LTTB, 2013)
- Datawrapper Blog — "What to consider when choosing colors for data visualization"; "Your Friendly Guide to Colors in Data Visualisation"
- Amelia Wattenberger — "Accessible charts" / Chartability (Frank Elavsky) — https://chartability.fizz.studio
- Refactoring UI — Charts and data-dense layout хэсэг
