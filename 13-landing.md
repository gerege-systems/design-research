[← Индекс руу буцах](README.md)

# Landing / маркетингийн сайт

SaaS product site, agency site, one-pager-т хамаарна. Landing-ийн нэг л зорилго — зочныг **нэг үйлдэл** рүү хөтлөх (бүртгүүлэх, demo захиалах, татах). Дизайны бүх шийдвэр энэ нэг үйлдлийг хурдан, ойлгомжтой, итгэлтэй болгоход үйлчилнэ. Өнгө, типограф, зай, компонентын суурь дүрэм бусад файлд — энд зөвхөн landing-д өвөрмөц дүрмүүд.

## Хуудасны анатоми — дараалал

| # | Хэсэг | Заавал? | Тайлбар |
|---|---|---|---|
| 1 | Nav | Тийм | Лого + ≤5 линк + CTA |
| 2 | Hero | Тийм | Гарчиг, дэд гарчиг, CTA, visual |
| 3 | Social proof strip | Сонголт | Лого мөр эсвэл 3 тоо |
| 4 | Features / benefits | Тийм | 3 эсвэл 6 card |
| 5 | How it works | Сонголт | 3 алхам (≤4), дугаартай, алхам бүр 1 гарчиг + 1 өгүүлбэр |
| 6 | Testimonials | Сонголт | 1-3 бодит сэтгэгдэл |
| 7 | Pricing | SaaS-д тийм | Agency-д «Холбогдох» орлоно |
| 8 | FAQ | Сонголт | 5-8 асуулт |
| 9 | Final CTA | Тийм | Hero-гийн CTA-г давтана |
| 10 | Footer | Тийм | Баганууд, legal, холбоо барих |

- **Нэг хэсэг = нэг санаа, нэг h2.** Хоёр санаа багтаах гэж байвал хоёр хэсэг болго.
- One-pager-т 3, 5, 6-г хасаж болно; 1, 2, 4, 9, 10 хэзээ ч хасагдахгүй.

## Above the fold (hero)

- **Гарчиг нэг, ≤10 үг, feature биш үр ашиг** («AI-тай» биш «Тайлангаа 5 минутад гарга»). h1 нь хуудсанд ганц.
- Дэд гарчиг ≤25 үг: хэн, юу, яаж — гурвыг хариулна.
- **CTA нэг primary** + заавал биш нэг secondary ghost («Demo үзэх»). Гурав дахь товч — алдаа.
- Visual-ийн эрэмбэ: бодит product screenshot > өөрийн illustration > stock photo. Stock-ийн хүний гар барилцсан зураг — хориотой.
- Hero-г бүтэн дэлгэц (`h-screen`, `dvh`-ийн 100%) болгож бүү хүчил: desktop 560-720px орчим, контент дагаж `min-height`. Доор хэсэг байгаа нь fold дээр мэдрэгдэх ёстой.
- Autoplay видео, hero carousel хэрэглэхгүй (2 дахь slide-ыг <1% хүн хардаг); видео зайлшгүй бол `muted loop playsinline` + poster.

## Navigation

- Линк ≤5 + 1 CTA товч (primary). Хэрэв 6+ хэрэгтэй бол dropdown-д бүү нуу — хуудсаа хуваа.
- Sticky, өндөр 56-72px; scroll эхлэхэд `box-shadow`/доод border гарна (04-visual-details.md-г үз). Mobile: hamburger → sheet, CTA товч доор үлдэнэ.

## Section rhythm

- Босоо padding: desktop **64-96px**, mobile **48-64px**. Хэсэг хооронд зайг нэг token-оор (`--space-section`) удирд.
- Контейнер `max-width` 1152-1280px (03-spacing-layout.md-г үз); текст блок `max-width: 65ch`.
- Хэсэг ялгах: background-ийг `bg` / `surface` ээлжлүүл — border, divider хэрэггүй.
- Хэсэг бүр: h2 (32-48px) → 1 өгүүлбэр тайлбар → контент. Eyebrow (12-13px uppercase muted) хэрэглэвэл бүх хэсэгт, эсвэл хаана ч үгүй.

## CTA иерархи

- Primary CTA урт хуудсанд **3-4 удаа** давтагдана: hero, дунд (features-ийн дараа эсвэл pricing), final CTA, nav.
- **Label бүх газар ижил** («Үнэгүй эхлэх» гэвэл хаа сайгүй яг тэр үгээр). Өөр үгтэй товч = өөр үйлдэл гэж уншигдана.
- Товчны хэмжээ marketing горим: `xl` — өндөр 44px, font 16px, touch target ≥44×44 (06-components.md-г үз).
- Final CTA хэсэг: h2 + 1 мөр + primary товч, өөр юу ч үгүй. Risk-reversal мөр («Карт шаардахгүй · 14 хоног үнэгүй») товчны доор 13-14px.

## Social proof

- Лого мөр: greyscale (`filter: grayscale(1)`, opacity 0.6-0.8), **өндөр жигд 24-32px**, 4-8 лого, hover-т өнгө орж болно.
- Testimonial: бодит нэр + албан тушаал + компани + зураг (40-48px тойрог). Нэргүй «Хэрэглэгч» — бүү оруул.
- Тоо контексттэй: «10,000+» биш «10,000+ идэвхтэй хэрэглэгч, 2024 оноос». 3 тоо хүрэлцээтэй, 4-өөс дээш уншигдахгүй.

## Features / benefits

- **3 эсвэл 6** (1×3, 2×3 grid). 4 бол 2×2 зөвшөөрнө; 12 feature-ийн хана — алдаа, хоёр хуудас болго.
- Card: icon (24-32px, нэг style) + гарчиг (18-20px, 600, үр ашгаар: «Real-time sync» биш «Хаанаас ч хамт ажилла») + **2 мөр** тайлбар.

## Pricing table

- Plan **≤4**; 3 нь оновчтой. Нэгийг «Санал болгох» гэж тэмдэглэнэ: badge + border accent эсвэл бага зэрэг том/өргөгдсөн.
- Сар/жил toggle — жилийн хөнгөлөлтийг тоогоор харуул («2 сар үнэгүй», «-20%»), сонголтыг URL/ storage-д санах нь сонголт.
- Үнэ том (36-48px), `font-variant-numeric: tabular-nums`; валют, хугацаа тодорхой (`49,000₮ /сар`). ₮ ард, зайгүй + мянгатын таслал (09-localization-mn.md-г үз).
- Feature жагсаалт plan тус бүрд **≤8 мөр**, дараа нь «Бүгдийг харьцуулах» линк → бүрэн хүснэгт.
- Plan бүрд CTA: санал болгоход primary, бусдад secondary.

## FAQ

- 5-8 асуулт, хариулт бүр ≤3 өгүүлбэр. Худалдан авалтад саад болох асуултууд (үнэ, гэрээ, цуцлалт, дата аюулгүй байдал) эхэнд.
- Accordion нь native `<details><summary>` — JS хэрэггүй, keyboard-д бэлэн, хайлтад индекслэгдэнэ.

```html
<details>
  <summary>Хэзээ ч цуцалж болох уу?</summary>
  <p>Тийм. Тохиргооноос нэг товчоор, үлдсэн хугацаа хүртэл үйлчилнэ.</p>
</details>
```

## Footer

- 3-5 багана: Бүтээгдэхүүн / Компани / Нөөц / Legal; багана тус бүр ≤6 линк.
- Legal линк заавал: Privacy, Terms (шаардлагатай бол Cookie); © он + нэр. Холбоо барих имэйл **олон нийтийн хаяг** (hello@, support@) — дотоод/хувийн хаягийг хэзээ ч тавихгүй.
- Хэл солих (EN/MN) footer эсвэл nav-д; public хуудсанд **URL эх сурвалж** (`/mn/`, `/en/`), cookie/localStorage биш (09-localization-mn.md).

## Landing дээрх форм

- Hero-д **нэг талбар** (имэйл) + товч, эсвэл тусдаа форм **≤4 талбар**. 5-аас дээш талбар бол multi-step эсвэл тусдаа хуудас.
- Inline validation blur дээр, алдаа талбарын доор (06-components.md-г үз). Submit дарсны дараа success төлөв форм байрандаа гарна — өөр хуудас руу үсрэхгүй.
- `type="email"` + `autocomplete="email"`; spam хамгаалалт honeypot/turnstile — CAPTCHA зураг бүү ашигла.

## Imagery ба motion

- Product screenshot бодит UI, бодит мэт дата («Lorem ipsum», «User 1» харагдахгүй). Illustration, icon, browser frame — тус бүр нэг л style (04-visual-details.md-г үз).
- Формат WebP/AVIF, `<picture>`-ээр fallback; зураг бүрт `width`/`height` эсвэл `aspect-ratio` — CLS-ээс хамгаална.
- **Hero зураг = LCP**: `<link rel="preload">` + `fetchpriority="high"` + `loading="eager"`; бусад бүх зураг `loading="lazy"`.
- Reveal анимаци: opacity + translateY(12-16px), 240ms (`--duration-slow`), **нэг л удаа** (`IntersectionObserver`, дахин scroll-д дахихгүй). `prefers-reduced-motion: reduce` → reveal-гүй, бүгд шууд харагдана (05-motion.md-г үз).

## Performance budget

| Хэмжүүр | Босго |
|---|---|
| LCP | ≤2.5s |
| INP | ≤200ms |
| CLS | ≤0.1 |
| Нийт JS (gzip) | ≤100-150KB |
| Font файл | ≤4 woff2 (≤2 weight × latin + cyrillic-ext; `font-display: swap`) |
| Third-party script | Зөвхөн analytics, `defer`/`async`, consent-ийн дараа |

- Chat widget, heatmap, A/B tool бүр 100-300KB нэмдэг — хэрэгтэй бол эхний interaction-ийн дараа ачаал.

## SEO / meta

- `<h1>` нэг; `<title>` ≤60 тэмдэгт («Бүтээгдэхүүн — үр ашиг» хэлбэрээр); `meta description` ≤155.
- Open Graph: `og:title`, `og:description`, `og:image` **1200×630** (текст төвд, захын 10%-д чухал зүйл үгүй), `twitter:card=summary_large_image`.
- `<link rel="canonical">`, `sitemap.xml`, `<html lang="mn">`/`"en"` зөв; EN/MN хувилбарт `hreflang` (`mn`, `en`, `x-default`) хоёр талдаа харилцан заана.
- Structured data (JSON-LD): `Organization` (лого, sameAs), `WebSite`, FAQ байвал `FAQPage`; SaaS-д `SoftwareApplication` + `Offer` сонголт.

## Итгэл, dark mode

- HTTPS, Privacy/Terms хуудас бодит агуулгатай, бодит хаяг/имэйл. Compliance badge зөвхөн бодит байвал (SOC 2, ISO) — чимэглэлийн «Secure» icon бүү тавь.
- Dark mode marketing-д заавал биш. **Нэг default сонго** (ихэвчлэн light); toggle зөвхөн бүтээгдэхүүн өөрөө dark mode-той бол. Дэмжвэл screenshot, лого, OG зураг бүгд хоёр хувилбартай (01-color.md-г үз).

## Anti-pattern

- Gradient текст хаа сайгүй (зөвхөн hero h1-ийн нэг үгэнд, эсвэл огт үгүй) · glassmorphism, blur давхарласан card (contrast унана).
- 3 өрсөлдсөн CTA («Эхлэх» · «Demo» · «Татах» зэрэгцээ).
- Тодорхойгүй copy: «Бизнесээ дараагийн түвшинд», «Innovative solution» — тоо, үйл үг, объекттой болго.
- Stock гар барилцах зураг · нээгдмэгц popup/newsletter modal · exit-intent popup.
- Infinite hero анимаци, хүнд parallax, scroll-jacking · hero carousel · autoplay дуутай видео.
- Cookie banner дэлгэцийн хагасыг халхалсан («Татгалзах» нь «Зөвшөөрөх»-тэй тэнцүү тод байх).

## Pre-launch checklist

1. h1 нэг, ≤10 үг, үр ашиг хэлсэн; title ≤60, description ≤155.
2. Primary CTA нэг label, 3-4 удаа давтагдсан; hero-д ≤2 товч.
3. Lighthouse mobile: LCP ≤2.5s, INP ≤200ms, CLS ≤0.1; JS ≤150KB gz.
4. Hero зураг preload + `fetchpriority="high"`, бусад lazy; бүх зурагт хэмжээ/aspect-ratio.
5. Font ≤4 woff2 (≤2 weight), swap, cyrillic-ext subset; third-party зөвхөн analytics, defer.
6. OG 1200×630 зураг, canonical, sitemap, `lang`, EN/MN `hreflang`, JSON-LD validator-т алдаагүй.
7. Privacy, Terms, холбоо барих бодит; дотоод имэйл хаяг байхгүй; © он зөв.
8. Keyboard-аар nav → CTA → форм → FAQ бүгд хүрнэ, focus харагдана; contrast ≥4.5:1 (07-accessibility.md-г үз).
9. Форм: validation, success төлөв, spam хамгаалалт ажиллаж, имэйл бодитоор ирж байгаа.
10. 320 / 375 / 768 / 1280 / 1920px-д overflow, текст давхцал, зураг тасралт байхгүй; reduced-motion-д анимаци унтарна.

## Эх сурвалж

- Nielsen Norman Group — "Carousel Usability: Designing an Effective UI for Websites with Content Overload", "Popups: 10 Problematic Trends and Alternatives", "Hamburger Menus and Hidden Navigation Hurt UX Metrics", "Accordions on Desktop: When and How to Use" (nngroup.com/articles/)
- web.dev — "Core Web Vitals" (web.dev/articles/vitals), "Optimize Largest Contentful Paint" (web.dev/articles/optimize-lcp), "Optimize Cumulative Layout Shift" (web.dev/articles/optimize-cls), "Fetch Priority API" (web.dev/articles/fetch-priority)
- MDN — `<details>`, `<picture>`, `aspect-ratio`, `rel=preload`, `hreflang`
- Google Search Central — "Structured data: FAQPage, Organization", "Localized versions of your pages" (developers.google.com/search/docs); Schema.org — Organization, WebSite, FAQPage, SoftwareApplication; The Open Graph protocol (ogp.me)
- WCAG 2.2 — 1.4.3 Contrast (Minimum), 2.2.2 Pause, Stop, Hide, 2.3.3 Animation from Interactions, 2.4.6 Headings and Labels, 3.1.1 Language of Page
- Refactoring UI (Wathan & Schoger) — hierarchy, "Start with too much white space", imagery
- Julian Shapiro — "Landing Page Guide" (julian.com/guide/growth/landing-pages)
