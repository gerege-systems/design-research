[← Индекс руу буцах](README.md)

# Типограф

## Font family — 1-2, дээд тал нь 3

- **1 font** — хамгийн аюулгүй: нэг sans-serif-ийн weight-үүдээр (**400/500/600** — 700 хэрэглэхгүй) бүх иерархийг гаргана. Default: **Geist** (кирилл + Ө/Ү `cyrillic-ext`-д ✓); Inter зөвхөн stack-ийн fallback, default биш.
- **2 font** — сонгодог хослол: гарчигт display/serif, body-д sans. Маркетинг, контент сайтад сайн.
- **3 дахь нь** зөвхөн monospace (код, дугаар, table-ийн тоо).

Үүнээс олон болбол сайт «эвлүүлэг» шиг харагддаг.

## Type scale — 5-8 шатлал

Дур мэдэн px өгөхийн оронд **modular scale**: суурь хэмжээг тогтмол харьцаагаар үржүүлнэ.

| Scale | Харьцаа | Хэрэглээ |
|---|---|---|
| Minor third | 1.2 | Dashboard, data-нягт UI |
| Major third | 1.25 | Ерөнхий вэб апп |
| Perfect fourth | 1.333 | Маркетинг, landing page |
| Golden ratio | 1.618 | Том hero-той editorial сайт |

Жишээ: base 16px × 1.25 → `12.8 → 16 → 20 → 25 → 31 → 39 → 49`. Санамж: Tailwind-ийн default scale (12/14/16/18/20/24/30/36/48/60/72) нь modular **биш**, гараар сонгосон утгууд — modular scale хэрэгтэй бол `@theme`-д өөрөө тодорхойл.

**Default (эргэлзвэл):** апп/dashboard **1.2 @ 14px**, маркетинг/контент **1.25 @ 16px**; нэг бүтээгдэхүүнд **≤8 хэмжээ** ашиглана. gerege-ui-ийн бодит scale 08-design-tokens.md-д.

## Font-size-ийн нэгж: px биш rem

- `1rem = 16px` (хэрэглэгчийн browser тохиргоо). rem-ээр бичсэн текст хэрэглэгчийн default size-ийг дагаж томордог — accessibility-ийн үндсэн шаардлага.
- `html { font-size: 62.5% }` (1rem=10px) трюкийг хэрэглэхгүй — хэрэглэгчийн тохиргоог гажуудуулдаг.
- `em` нь эцгээсээ хамаарч давхарласан үед үржигддэг — зөвхөн component-дотоод харьцаанд (icon текстээ дагах гэх мэт).

## Элемент тус бүрийн түгээмэл утгууд

| Хэрэглээ | Хэмжээ | Тайлбар |
|---|---|---|
| Caption, badge, table header | 12px (0.75rem) | **Доод хязгаар** — бүх UI текстэд; ганц үл хамаарах: chart-ийн tick label 11px (12-data-viz.md) |
| Secondary/UI text, dashboard body | 13-14px | Data-нягт UI-ийн ажлын морь |
| Body (контент сайт) | 16-18px | Урт текстэд 16-аас доошгүй |
| H4 / card title | 16-18px, 600 weight | |
| H3 | 20-24px | |
| H2 | 24-31px | |
| H1 / page title | 31-39px | Апп дотор 24-30px хангалттай |
| Hero (landing) | 48-72px | clamp()-тай fluid |

Апп UI (13-14px суурь) ба контент/маркетинг (16-18px суурь) хоёр өөр «горим» — нэг проект дотор зэрэгцэж болно (жишээ: admin 14px, landing 16px).

iOS 16px-ээс жижиг input-д автоматаар zoom хийдэг тул **форм дээр заавал 16px+**.

## Fluid typography — clamp()

```css
h1 { font-size: clamp(2rem, 1rem + 3vw, 3.5rem); }
```

- Гурван утга: доод хязгаар, viewport-хамааралт утга, дээд хязгаар.
- Дунд утгад заавал `rem + vw` холимог — цэвэр `vw` бол zoom-д томордоггүй тул WCAG-д унадаг.
- Зөвхөн display түвшинд (H1, H2, hero); body, товч, форм — fixed. Бүгдийг fluid болговол иерархи шахцалдана.
- [Utopia](https://utopia.fyi) — бүтэн fluid scale бодох де-факто калькулятор.

## Иерархи нь size-ээс гадна weight + color

Бүх ялгааг хэмжээгээр гаргах гэж 10 шатлал үүсгэхгүй. Ижил 14px текст 400/muted vs 600/foreground байхад л хоёр өөр түвшин болно. Текстийн өнгөний 3 түвшин: foreground / muted / subtle.

## Line-height ба мөрийн урт

- Body: **1.5-1.7**
- Гарчиг: **1.1-1.3** (том тусмаа бага)
- Мөрийн урт: 60-75 тэмдэгт — `max-width: 65ch`

## Letter-spacing / optical size

- Том гарчигт агшаана: hero-д `letter-spacing: -0.02em`
- Жижиг caption-д (12px) `+0.01em` орчим
- Variable font-ийн `opsz` axis (Inter Display cut гэх мэт) үүнийг автоматаар хийдэг.

## Ерөнхий зөвшилцөл

**1 font family (+mono) · 6-8 size · 3 weight (400/500/600) · 2-3 color түвшин** — үүнээс цомхон систем бараг бүх UI-д хүрэлцдэг.

Албан ёсны стандарт гэвэл WCAG л бий (хэмжээ биш contrast + 200% zoom шаарддаг). Material Design (13 түвшин), Apple HIG (11 түвшин) нь convention; практикт 6-8 л ашиглагддаг.

## Шрифт ачаалалт

- **`font-display: swap`** — body/UI шрифтэд default. Чимэглэлийн, display-only шрифтэд `optional` (100ms-д ирэхгүй бол fallback-аар үлдэнэ, CLS үүсгэхгүй).
- Preload зөвхөн **1-2 критик файл** (body 400 + 600 жишээ нь): `<link rel="preload" href="/fonts/geist-400-cyrillic-ext.woff2" as="font" type="font/woff2" crossorigin>`. `crossorigin`-гүй бол хоёр удаа татдаг. 3-аас олон preload нь бусад ресурсийг хойшлуулна.
- **Subsetting**: `unicode-range`-ээр latin + cyrillic тус тусад нь файл болгож, хэрэгтэйг нь л татуулна. Бүтэн Inter ~300KB vs latin+cyrillic subset ~60-80KB woff2.
- **Variable font** — 4 static файлын оронд нэг файл (weight 100-900 + opsz); ихэвчлэн нийт байт ч бага. `font-weight: 100 900` гэж `@font-face`-д зарла.
- **Metric-compatible fallback** — CLS-ийг 0 болгох аргачлал:

```css
@font-face {
  font-family: "Geist Fallback";
  src: local("Arial");
  size-adjust: 107%;
  ascent-override: 90%;
  descent-override: 22%;
  line-gap-override: 0%;
}
body { font-family: Geist, "Geist Fallback", Inter, system-ui, sans-serif; }
```

Утгыг гараар биш — Fontaine / Capsize / `next/font` автоматаар бодно.

- **Self-host** давуу: Google Fonts хост нь 2020-оос хойш browser cache-ээ хуваалцдаггүй тул хурдны давуу тал байхгүй; CSP-д нэмэлт `font-src` нээх шаардлагагүй; GDPR-ийн IP дамжуулалтын асуудалгүй. `next/font`, Fontsource хоёулаа self-host хийдэг. Google Fonts зөвшөөрөгдөнө — зөвхөн `preconnect` + `display=swap` + `cyrillic-ext` subset-тэй.
- `font-synthesis: none` — 600 файл байхгүй үед browser «хуурамч bold/italic» зурахыг хориглоно; дутуу weight-ийг нүдээр илрүүлнэ.
- Нийт шрифт байт: **≤100KB** критик замд; **≤4 woff2 файл** (≤2 weight × latin + cyrillic-ext subset). Weight 400/500/600-аас өөрийг ачаалахгүй.

**Кирилл**: сонгосон шрифт бүр кирилл (U+0400-04FF) + монгол-тусгай **Ө (U+04E8/04E9), Ү (U+04AE/04AF)** үсгийг агуулж байгааг заавал шалга. Geist-д бий (`cyrillic-ext` subset, 2026-08-20 шалгасан) — гэхдээ subset-ээ ачаалахаа мартвал fallback руу унана; зарим display font-д огт байхгүй. Дутуу бол browser өөр шрифтээс нөхөж «холимог» харагдана. Дэлгэрэнгүй ба жагсаалт: 09-localization-mn.md-г үз.

## Эх сурвалж

- WCAG 2.2 — SC 1.4.4 Resize Text, 1.4.10 Reflow, 1.4.12 Text Spacing — w3.org/TR/WCAG22/
- MDN — `font-display`, `unicode-range`, `size-adjust`, `ascent-override`, `font-synthesis`, `clamp()`, `<link rel="preload">` — developer.mozilla.org/en-US/docs/Web/CSS/@font-face
- web.dev — «Best practices for fonts»; «Reduce web font size»; «Optimize Cumulative Layout Shift»
- Utopia — Fluid type scale calculator — utopia.fyi/type/calculator
- Type Scale — typescale.com (modular scale харьцаанууд)
- Material 3 — Typography (type scale tokens); Apple HIG — Typography
- Refactoring UI — «Establish a type scale», «Use good fonts»
- Nielsen Norman Group — «Legibility, Readability, and Comprehension»
- Fontaine (unjs), Capsize (seek-oss) — fallback metric tooling
