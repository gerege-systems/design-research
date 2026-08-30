[← Индекс руу буцах](README.md)

# Accessibility

Албан ёсны цорын ганц «стандарт» бол **WCAG 2.2** (AA түвшин нь практик жишиг). Бусад нь convention.

## Contrast

- Энгийн текст (<24px): **≥4.5:1**
- Том текст (≥24px эсвэл ≥18.7px bold): **≥3:1**
- UI компонент, icon, input border (non-text): **≥3:1** — тэр элемент суудаг **бүх** фон дээр (`--background`, `--background-muted`, card, sidebar); нэг фон дээр л шалгаад орхихгүй.
- Disabled төлөв contrast шаардлагаас чөлөөлөгддөг — гэхдээ уншигдахуйц байлга.
- Шалгах: WCAG ratio — Chrome DevTools color picker, axe DevTools; APCA (нэмэлт, WCAG 3 draft) — apcacontrast.com эсвэл `apca-w3` npm (`import { APCAcontrast, sRGBtoY } from 'apca-w3'`) — token бүрийг CI-д lint хийх (15-checklist.md).

## Zoom ба хэмжээ

- **200% zoom-д** контент алдагдахгүй, хэвтээ scroll үүсэхгүй байх ёстой — rem + fluid layout үүнийг шийднэ.
- WCAG font-size-ийн доод хэмжээ заадаггүй; харин zoom + reflow шаарддаг.
- `user-scalable=no`, `maximum-scale=1` хэзээ ч тавихгүй.

## Keyboard

- Бүх interactive элемент Tab-аар хүрэгдэж, Enter/Space-ээр ажиллана.
- **Focus-visible** төлөв: `outline: 2px solid var(--ring); outline-offset: 2px` — accent өнгө, хөрш өнгөнөөсөө ≥3:1. Accent-fill товч (primary) дээр ганц ring фонтойгоо нийлнэ → **давхар ring**: `box-shadow: 0 0 0 2px var(--background), 0 0 0 4px var(--ring)`. `outline: none`-ыг орлуулах юмгүйгээр бүү бич; `:focus` биш `:focus-visible`.
- Tab дараалал нь визуал дарааллыг дагана; `tabindex` эерэг утгаар бүү хэрэглэ.
- Modal — focus trap; хаагдахад анхны trigger рүү focus буцаана.
- Skip link («Гол контент руу») — nav ихтэй хуудсанд.

## Semantic HTML

- `div`+onClick биш `button`; `span` биш `a` (навигацид).
- Heading дараалал алгасахгүй: h1 → h2 → h3 (харагдац нь class-аар, бүтэц нь tag-аар).
- Landmark-ууд: `header/nav/main/footer` — screen reader-ийн навигаци.
- Icon-only товчид `aria-label`; чимэглэлийн icon-д `aria-hidden="true"`.
- Форм: `label[for]` заавал холбоотой; алдааг `aria-describedby`-гаар талбартай холбо.

## Өнгөнөөс хамаарахгүй байх

- Статусыг зөвхөн өнгөөр илэрхийлэхгүй — icon/label давхар (ногоон/улаан color blindness-д ижил харагдаж болно).
- Линк текст дотроо зөвхөн өнгөөр ялгарч байвал underline нэм.

## Touch

- Pointer target: desktop/нягт dashboard-д **≥24×24px** (WCAG 2.2 SC 2.5.8, AA — бидний доод хязгаар); touch-first дэлгэц, landing CTA-д **≥44×44px** (HIG 44 / M3 48 convention). Жижиг текстэн линк ч padding-аар хүрнэ.
- Target хоорондын зай ≥8px — андуурч дарахаас сэргийлнэ.

## Motion

- `prefers-reduced-motion: reduce`-ийг хүндэл (05-motion.md-г үз).
- Автоматаар тоглодог video/carousel-д зогсоох боломж заавал.

## Шалгах хэрэгслүүд

- Lighthouse (Chrome DevTools) — эхний шүүлт
- axe DevTools — дэлгэрэнгүй
- Гараар: Tab-аар л бүх хуудсаа тойрч үзэх — хамгийн хурдан бодит тест
- VoiceOver (macOS: Cmd+F5) — жинхэнэ screen reader туршилт

## WCAG 2.2-ийн шинэ шалгуурууд

2023-10-д нэмэгдсэн 9 шалгуур; AA-д хамаарах нь 6:

| SC | Түвшин | Дүрэм |
|---|---|---|
| 2.4.11 Focus Not Obscured (Minimum) | AA | Focus-той элемент sticky header/footer, cookie banner-ийн **ард бүрэн нуугдахгүй**. `scroll-padding-top: var(--header-h)` тавь. |
| 2.4.12 Focus Not Obscured (Enhanced) | AAA | Огт нуугдахгүй (хэсэгчлэн ч). |
| 2.4.13 Focus Appearance | AAA | Focus индикатор ≥2px өргөн, өмнөх төлөвтэйгөө ≥3:1 contrast. AAA боловч практикт 2px outline + offset-оор шууд хангагдана. |
| 2.5.7 Dragging Movements | AA | Drag шаарддаг үйлдэл бүрт (reorder, slider, kanban) **drag-гүй хувилбар**: дээш/доош товч, сум товчлуур, тоон input. |
| 2.5.8 Target Size (Minimum) | AA | Target **≥24×24 CSS px**, эсвэл 24px тойрог хөрш target-тай давхцахгүй. Мөрөн дэх inline линк, browser default control чөлөөлөгддөг. |
| 3.2.6 Consistent Help | A | Тусламж (chat, утас, FAQ линк) хуудас бүр дээр **ижил байрлалд**. |
| 3.3.7 Redundant Entry | A | Нэг flow дотор ижил мэдээллийг **хоёр дахин асуухгүй** — өмнө оруулсныг auto-fill эсвэл сонгох боломж («Хүргэлтийн хаяг = төлбөрийн хаяг» checkbox). `autocomplete` attribute зөв тавь. |
| 3.3.8 Accessible Authentication (Minimum) | AA | Нэвтрэхэд **cognitive test** (captcha-д дүрс таах, нууц үг цээжээр бичих) цорын ганц арга байж болохгүй. Paste-ийг бүү хориглоо (`onpaste="return false"` ✗), password manager ажиллана (`autocomplete="current-password"`), passkey/OTP/magic link хувилбар өг. |
| 3.3.9 Accessible Authentication (Enhanced) | AAA | Объект таних тест ч хориглоно. |

Хасагдсан: 4.1.1 Parsing (HTML validity) — WCAG 2.2-д хүчингүй.

## ARIA ба динамик контент

- **ARIA-гүй нь буруу ARIA-аас дээр.** Native элемент (`button`, `a`, `select`, `dialog`, `details`) байвал түүнийг хэрэглэ; ARIA нь HTML-д байхгүй семантикийг л нөхнө. `<button role="button">` ✗, `<div role="button">` нь keyboard/focus-ийг гараар нөхөх шаардлагатай.
- Нийлмэл компонентыг **WAI-ARIA Authoring Practices Guide (APG)** загвараар: Tabs, Menu/Menu button, Dialog (Modal), Combobox, Listbox, Disclosure, Tooltip — keyboard дараалал (Arrow, Home/End, Esc) бүгд тэнд заасан. Өөрөө зохиохгүй; Radix/React Aria/Headless UI яг APG-г хэрэгжүүлдэг.
- **Live region**: async үр дүн, toast, «3 зүйл нэмэгдлээ» мэдэгдлийг `aria-live="polite"` бүстэй элементэд **бич** (элементийг өөрийг нь динамикаар үүсгэвэл уншигдахгүй — бүс нь хуудас ачаалахад DOM-д байх ёстой). Товчлол: `role="status"` = polite, `role="alert"` = assertive (зөвхөн алдаа, хугацаа дуусах гэх мэт яаралтай). Хуудсанд нэг-хоёр live region хангалттай.
- Modal нээлттэй үед арын контентод **`inert`** attribute — focus trap + screen reader-ээс хоёуланд нь нуугдана. `<dialog>.showModal()` үүнийг автоматаар хийдэг.
- Дэлгэцэнд харагдахгүй боловч screen reader уншдаг текст:

```css
.sr-only { position: absolute; width: 1px; height: 1px; padding: 0; margin: -1px;
  overflow: hidden; clip: rect(0,0,0,0); white-space: nowrap; border: 0; }
```

`display:none`/`visibility:hidden` нь screen reader-ээс ч нуудаг — ялгааг санах.

- Disclosure (accordion, dropdown trigger): товчинд `aria-expanded="true|false"` + `aria-controls="panel-id"`; төлөв солигдоход attribute-ийг шинэчил.
- Навигацид идэвхтэй хуудсыг `aria-current="page"` — зөвхөн `.active` class биш.
- Breadcrumb/pagination: `<nav aria-label="Breadcrumb">` — олон `<nav>` байвал тус бүрийг `aria-label`-аар ялга.
- `aria-label`-ийг **interactive бус** `div`/`span`-д бүү тавь — ихэнх screen reader үл тоомсорлоно; тайлбар хэрэгтэй бол харагдах текст эсвэл `aria-describedby`.
- `aria-hidden="true"` дотор focus-той элемент байж болохгүй.
- Хуудас бүрт **яг нэг `<h1>`**, хуудасны гарчиг (`<title>`) түүнтэй таарна; SPA route солигдоход `<title>` + focus-ийг шинэчил.
- `<html lang="mn">` — screen reader-ийн дуудлага, hyphenation, шрифт сонголт үүнээс хамаарна; хэл холимог хэсэгт `lang` attribute элемент дээр (09-localization-mn.md-г үз).
- Шалгах: axe DevTools-ийн ARIA дүрмүүд + Accessibility tree (Chrome DevTools → Accessibility tab) — role/name/state гурвууланг элемент бүрт харж баталгаажуул.

## Эх сурвалж

- WCAG 2.2 Recommendation (2023-10-05) — w3.org/TR/WCAG22/ ; «What's New in WCAG 2.2» — w3.org/WAI/standards-guidelines/wcag/new-in-22/
- Understanding WCAG 2.2 — SC 1.4.3, 1.4.11, 1.4.4, 1.4.10, 2.1.1, 2.4.7, 2.4.11, 2.4.13, 2.5.7, 2.5.8, 3.2.6, 3.3.7, 3.3.8 — w3.org/WAI/WCAG22/Understanding/
- WAI-ARIA Authoring Practices Guide (APG) — Patterns — w3.org/WAI/ARIA/apg/patterns/
- WAI-ARIA 1.2 — «First Rule of ARIA Use» — w3.org/TR/using-aria/
- MDN — ARIA live regions; `inert`; `aria-current`; `aria-expanded`; `<dialog>` — developer.mozilla.org
- web.dev — Learn Accessibility; «Accessible tap targets»
- Apple HIG — Accessibility; Layout (44pt target); Material 3 — Accessibility (48dp target)
- Deque — axe-core rules — dequeuniversity.com/rules/axe/
- Tailwind CSS — `sr-only` utility (source of the snippet)
