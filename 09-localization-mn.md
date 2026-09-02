[← Индекс руу буцах](README.md)

# Монгол хэл ба локалчлал

## Яагаад

Ихэнх UI kit, mockup, font English-ээр бүтээгддэг. Монгол кирилл нь **урт үг + 2 нэмэлт үсэг (Ө, Ү)** + өөр тоо/огнооны дадалтай тул шууд орчуулахад товч хагарч, үсэг `□` болж, огноо буруу гардаг. Доорх дүрмүүд эдгээрийг эхнээс нь хаана. Font scale, хэмжээ (02-typography.md-г үз), компонентын суурь (06-components.md-г үз) энд давтагдахгүй.

## Font — кирилл, ялангуяа Ө/Ү бүрхэлт

| Font | Кирилл | Ө/Ү | Тэмдэглэл |
|---|---|---|---|
| Inter | ✓ | ✓ | Зөвхөн stack-ийн fallback; default биш |
| Roboto | ✓ | ✓ | Android system font |
| IBM Plex Sans | ✓ | ✓ | Mono хувилбар нь ч кирилл-тэй |
| Golos Text | ✓ | шалга | Орос гаралтай, кирилл сайн; Ө/Ү-г файл дээр баталгаажуул |
| Manrope | ✓ | шалга | Үндсэн кирилл бий; extended-ийг шалга |
| **Geist (default)** | ✓ | ✓ | Google Fonts хувилбар `cyrillic` + `cyrillic-ext` subset-тэй — Ө/Ү (U+04E8/04AE) `cyrillic-ext`-д, ₮ (U+20AE) `latin-ext`-д (2026-08-20 шалгасан). Self-host хийвэл ≥1.4 + cyrillic subset татах |
| system-ui | ✓ | ✓ | SF Pro / Segoe UI / Roboto бүгд бүрэн |

- **Font сонгохын өмнө `Өө Үү Ёё Hh` тест мөрийг тухайн font-оор render хийж хар** — fallback font руу унавал үсэг өөр өргөн/өндөртэй «үсрэлт» болж харагдана.
- Файлын түвшинд шалгах: [Wakamai Fondue](https://wakamaifondue.com) эсвэл [FontDrop!](https://fontdrop.info) — glyph хүснэгтээс U+04E8/U+04E9 (Ө/ө), U+04AE/U+04AF (Ү/ү) байгаа эсэх.
- Google Fonts-оос татахдаа `subset=cyrillic,cyrillic-ext` (эсвэл `next/font`-д `subsets: ['cyrillic', 'cyrillic-ext']`) — `latin` л сонговол Ө/Ү fallback руу унана.
- `font-family` stack-д кирилл-тэй fallback заавал: `Geist, Inter, system-ui, -apple-system, "Segoe UI", Roboto, sans-serif`.
- Bold/italic cut бүр тусдаа кирилл-тэй байх — 700 weight дээр л Ө алга болох тохиолдол бий.

## Урт үг, текстийн тэлэлт

- **English mockup-аас дизайн хийхдээ текст 30-40% уртасна гэж тооцоол** («Save» → «Хадгалах», «Settings» → «Тохиргоо», «Delete account» → «Бүртгэл устгах»). Nav, tab, товч, badge бүрийг хамгийн урт орчуулгаар нь шалга.
- Товч, tab, nav item-д `width` биш **`min-width`** — текст нь багтаж, богино бол тогтсон хэмжээнд үлдэнэ.
- Overflow-ийн шийдвэр — элементийн төрлөөр, таамаглахгүй:

| Элемент | Дүрэм |
|---|---|
| Товч, tab, nav item, badge | `white-space: nowrap` + `min-width` (эцэг нь `overflow-x: auto` эсвэл wrap) |
| Table cell | `overflow: hidden; text-overflow: ellipsis; white-space: nowrap` + заавал `title` |
| Body, тайлбар, toast текст | `overflow-wrap: break-word` (default) |
| Chip, URL, код, email | `overflow-wrap: anywhere` + `max-width` |
- Монгол үгийн дундаж урт кириллээр 7-8 үсэг, нийлмэл нэр томьёо 15-20 хүрдэг → body-д `overflow-wrap: break-word` default байг. **`anywhere` биш:** `anywhere` нь min-content хэмжээг нэг үсэг болтол багасгадаг тул нарийн дэлгэцэнд flex/table багана шахагдаж, "merge d", "+12 %" гэх мэт үг дундуур тасардаг (gerege-ui дээр 2026-08-21 бодитоор гарсан). `anywhere`-ийг зөвхөн тодорхой нарийн саванд (chip, URL, код) сонгож хэрэглэ.
- `hyphens: auto` нь `lang="mn"`-д найдваргүй — browser-уудад монгол hyphenation толь байхгүй. Гараар `&shy;` эсвэл огт хэрэглэхгүй.
- Grid/flex child-д `min-width: 0` — урт үг эцгээ тэлэхээс сэргийлнэ.

## Table

- Текст баганы `min-width` 120-160px (English-д 96-120px байдгаас ~30% илүү).
- Header-ийн uppercase-ийг кириллд болгоомжтой: «ТӨЛӨВ», «ҮЙЛДЭЛ» мэт богино үгэнд зүгээр, урт нэр томьёонд title case.
- Тасалж харуулахдаа `text-overflow: ellipsis` + заавал `title="бүтэн текст"` (эсвэл tooltip) — тасарсан монгол үг утгаа бүрэн алддаг.
- Огноо, дүн, утасны багана — `tabular-nums` + `white-space: nowrap`.

## EN/MN toggle

- **Бүх харагдах мөр i18n-ээр дамжина** — JSX/HTML дотор шууд бичсэн нэг ч string байхгүй (alt, placeholder, aria-label, toast, validation message орно).
- Хоёр хэл нэг translations object-д, ижил key бүтэцтэй; build эсвэл test үед missing key-г алдаа болго (`mn` дахь key бүр `en`-д байх, эсрэгээр ч мөн).
- Default хэл `mn`; toggle нь header-т текст хэлбэрээр («MN | EN»), туг зурагаар биш — туг нь улс, хэл биш.
- Хадгалах: **public/SEO хуудас → URL эх сурвалж** (`/mn/...`, `/en/...`; `?lang=` биш); **нэвтэрсэн апп → хэрэглэгчийн тохиргоо (сервер) + cookie** (SSR эхний render-д). localStorage-only хэзээ ч биш — share хийсэн линк, шинэ төхөөрөмж, SSR гурвууланд алдагдана.
- `<html lang="mn">` (EN хуудсанд `lang="en"`); хэл хольсон хэсэгт `<span lang="en">` — screen reader, hyphenation, font fallback бүгд үүнээс хамаарна.
- Олон хэлтэй public хуудсанд `<link rel="alternate" hreflang="mn" href=…>` + `hreflang="en"` + `hreflang="x-default"`.

## Огноо, цаг

- Монгол улс **UTC+8, DST байхгүй** (`Asia/Ulaanbaatar`). Серверт UTC хадгал, харуулахдаа нэг л газар хөрвүүл.
- Дэлгэцэнд **`yyyy-MM-dd HH:mm`** (2026-08-20 14:30), зөвхөн огноо бол `yyyy-MM-dd` — Монголд хамгийн түгээмэл, сортлогддог, хоёр хэлэнд ижил. Энэ л нэг формат: `2026.08.20`, «20 авг 2026» мэт хувилбар хэрэглэхгүй. Долоо хоног **Даваа**-аас эхэлнэ (calendar, date picker, week chart).
- `Intl.DateTimeFormat('mn')`-д CLDR өгөгдөл **бий** (Chrome/Node full-icu), гэхдээ гаралтын хэв маяг нь бидэнд тохирохгүй (`2026 оны 8-р сарын 20`, сар/гарагийн нэр урт, Node-ийн small-icu build-д root руу унадаг) → огноог **fixed format**-аар өөрөө угсар (`date-fns` `format(d, 'yyyy-MM-dd HH:mm')` эсвэл padStart); `Intl`-ийг зөвхөн relative time, timezone хөрвүүлэлтэд. Гарагийн товчлолоо өөрөө хадгал: Да, Мя, Лх, Пү, Ба, Бя, Ня.
- 24 цагийн систем, AM/PM байхгүй. Харьцангуй цаг («5 минутын өмнө») хэрэглэвэл hover дээр бүтэн огноог `title`-д өг.

## Тоо, валют

- Мянгатын тусгаарлагч `,`, бутархай `.` (Орос шиг зай/таслал биш): `1,250,000.50`.
- **Төгрөгийн тэмдэг `₮` — дүнгийн ард, зайгүй: `25,000₮`**; товчилсон `12.4M₮`. Банк, төрийн системд энэ давамгай; prefix хувилбар хэрэглэхгүй. ISO код хэрэгтэй бол `MNT`.
- **CLDR-тэй ухамсартай зөрнө.** `Intl.NumberFormat('mn-MN', {style:'currency', currency:'MNT'})` нь CLDR `mn` дагуу prefix (`₮ 25,000` / `MNT 25,000`, browser-оос хамаарна) буцаадаг. Suffix нь стандарт биш, Монголын банк/төрийн UI-ийн конвенц — тиймээс `Intl` currency style хэзээ ч шууд хэрэглэхгүй, нэг helper (`@gerege/ui`-ийн `formatMNT`, эсвэл `n.toLocaleString('en-US') + '₮'`) л ашигла. Гадаад (олон улсын) хэрэглэгчид зориулсан UI-д `MNT` кодтой prefix хэлбэрийг сонгож болно — нэг проектод нэг л хэлбэр.
- Төгрөг мөнгө (бутархай) практикт хэрэглэгддэггүй — хүүгийн тооцооноос бусад газарт 0 орон.
- Утас: **`+976 XXXX XXXX`** (8 орон, 4-4 бүлэглэнэ); дотоодод `XXXX-XXXX`. Input-д `type="tel" inputmode="tel" autocomplete="tel"`, E.164 (`+976XXXXXXXX`) хэлбэрээр хадгал.
- Хаяг: томоос жижиг рүү — Хот/аймаг → Дүүрэг/сум → Хороо/баг → Байр/гудамж → Тоот. Форм талбаруудаа энэ дарааллаар байрлуул; зип код 5 орон, шаардлагатай үед л асуу.

## Оролт (input)

- Хэрэглэгч кирилл/латин гар хооронд сэлгэдэг — email, URL, password-д `inputmode="email"`/`"url"` + `autocomplete` өг, бусад текст талбарт `lang="mn"` үлдээ.
- Кириллээр бичсэн email/username-ийг submit-ийн өмнө шалгаж ойлгомжтой алдаа өг («Латин үсгээр бичнэ үү»), чимээгүй хөрвүүлэхгүй.
- IME/compose үеэр `input` event дээр validation бүү ажиллуул — `compositionend` хүлээ (mobile Cyrillic keyboard autocorrect).
- `autocapitalize="off"` login талбарт — mobile гар кирилл эхний үсгийг томруулдаг.
- Регистр нэр, ТТД мэт талбарт хүлээн зөвшөөрөх тэмдэгтийн багцаа placeholder-т жишээгээр харуул (`УБ12345678`).

## Эрэмбэлэх, том үсэг, холимог бичиг

- Эрэмбэлэхдээ **`a.localeCompare(b, 'mn')`** — `<`/`>` харьцуулалт Ө, Ү-г цагаан толгойн төгсгөлд хаядаг. ICU-д `mn` collation байхгүй орчинд root руу унана → шаардлагатай бол «АБВГДЕЁЖЗИЙКЛМНОӨПРСТУҮФХЦЧШЩЪЫЬЭЮЯ» дарааллаар өөрийн comparator.
- `text-transform: uppercase` кириллд зөв ажиллана (ө→Ө, ү→Ү). Uppercase-д `letter-spacing: 0.04–0.06em`, **кириллд 0.06em** — кирилл том үсэг латинаас дөрвөлжин, нягт харагддаг. Ерөнхийдөө sentence case давуу (10-ux-writing-mn.md).
- Монгол өгүүлбэр доторх English нэр томьёо (API, token, Dashboard) — **ижил font, italics биш, хашилтгүй**. Italic кирилл олон font-д Latin-аас ялгаатай хэлбэртэй (т→m шиг) тул уншихад саад.
- Хашилт: **«...»** үндсэн, дотор нь „...“ хоёрдогч. Програмын `"..."` шулуун хашилтыг UI текстэд бүү үлдээ.
- Зураас: `—` (em dash) өгүүлбэрт, `-` зөвхөн нийлмэл үг/тоонд; тоон мужид `–` (en dash): `10–20`.
- RTL хэрэггүй — кирилл зүүнээс баруун, `dir` тохируулах шаардлагагүй.

## Монгол бичиг (босоо) — хамрах хүрээнээс гадуур

Энэ репо кирилл UI-д зориулагдсан. Монгол бичгийн дэмжлэг хэрэгтэй бол тусдаа судалгаа шаардлагатай: `writing-mode: vertical-lr` (мөр зүүнээс баруун тийш), font **Noto Sans Mongolian** (бусад font-д glyph байхгүй), Unicode Mongolian блокийн shaping (FVS, MVS тэмдэгтүүд) browser бүрт жигд биш, layout бүхэлдээ босоо flex/grid болж өөрчлөгдөнө.

## Хурдан шалгах

1. `Өө Үү Ёё` бүх font weight-д зөв font-оор гарч байна уу (fallback үсрэлтгүй)?
2. Хамгийн урт монгол label-аар товч/nav/tab хагарахгүй юу (`min-width`, `overflow-wrap`)?
3. JSX/HTML-д шууд бичсэн string үлдсэн үү, `mn`/`en` key бүрэн ижил үү?
4. `<html lang="mn">` + огноо `yyyy-MM-dd HH:mm`, UTC+8-аар гарч байна уу?
5. Дүн `25,000₮`, утас `+976 XXXX XXXX` нэг л форматаар уу?
6. Эрэмбэлэлт `localeCompare('mn')`, хашилт «...», холимог English italic биш үү?

## Эх сурвалж

- Unicode CLDR — `mn` locale (cldr.unicode.org; огноо/тооны хэв маяг яагаад fixed format-аар бичдэгийн эх)
- W3C Internationalization — «Text size in translation» (w3.org/International/articles/article-text-size)
- W3C Internationalization — «Language tags in HTML and XML», «Declaring language in HTML» (w3.org/International/questions/qa-html-language-declarations)
- MDN — `overflow-wrap`, `hyphens`, `writing-mode`, `text-transform`, `inputmode`, `autocomplete` (developer.mozilla.org)
- MDN — `Intl.DateTimeFormat`, `Intl.NumberFormat`, `String.prototype.localeCompare`
- Google — «hreflang: Tell Google about localized versions of your page» (developers.google.com/search/docs/specialty/international/localized-versions)
- Google Fonts — Inter, Roboto, IBM Plex Sans, Golos Text, Manrope font specimen хуудсууд (fonts.google.com), glyph/subset хэсэг
- Wakamai Fondue (wakamaifondue.com), FontDrop! (fontdrop.info) — font файлын glyph шалгагч
- Unicode — Cyrillic block U+0400–U+04FF chart (unicode.org/charts/PDF/U0400.pdf); Mongolian block U+1800–U+18AF
- Apple HIG — «Right to Left», «Inclusion» (localization хэсэг)
- Material 3 — «Typography: Applying type» (internationalization тэмдэглэл)
- WCAG 2.2 — 3.1.1 Language of Page, 3.1.2 Language of Parts, 1.4.4 Resize Text, 1.4.10 Reflow
