[← Индекс руу буцах](README.md)

# Компонентын жишиг

Энэ файл компонентын **анатоми** (хэмжээ, төлөв, бүтэц) хариуцна. Dashboard дахь **зан төлөв** (хайлт, pagination, confirmation, keyboard, destructive урсгал) — [11-dashboard-patterns.md](11-dashboard-patterns.md) эзэмшинэ; энд давтахгүй, холбоос л байна. Тоон default-ууд [00-defaults.md](00-defaults.md)-тай ижил.

## Агуулга

- [Товч](#товч-button) · [Форм](#форм) · [Table](#table-data-нягт-ui) · [Card](#card) · [5 төлөв](#компонентын-5-төлөв)
- [Modal](#modal--dialog) · [Toast](#toast--notification) · [Tabs](#tabs) · [Dropdown](#dropdown--menu) · [Tooltip vs Popover](#tooltip-vs-popover)
- [Badge](#badge--status) · [Avatar](#avatar) · [Tag/Chip](#tag--chip) · [Select/Radio/Checkbox](#select-vs-radio-vs-checkbox) · [Date/Time](#date--time) · [File upload](#file-upload)
- [Search](#search) · [Multi-step форм](#multi-step-форм) · [Onboarding](#onboarding--first-run) · [11-д шилжсэн сэдвүүд](#11-dashboard-patternsmd-д-эзэмшигдсэн-сэдвүүд)

## Товч (Button)

- **Иерархи 3 түвшин**: primary (accent fill) · secondary (outline/ghost) · tertiary (text-only). Нэг view-д primary **нэг л байх** ёстой. craftzbay-ui-ийн default variant `primary` хэвээр (солибол breaking) — тиймээс бусад товчинд `variant="secondary"`-г **ил бич**.
- Хэмжээ (өндөр): **sm 32 / md 36 / lg 40 / xl 44**. Default md; compact dashboard sm; xl = marketing CTA, touch-first.
- Padding: хэвтээ нь босоогоосоо ~2 дахин (md: 8px 16px).
- Төлөвүүд бүгд байх: default / hover / active / focus-visible / disabled / loading.
- Loading үед хэмжээ өөрчлөгдөхгүй — текстийг spinner-ээр солихдоо width хадгал; давхар submit боломжгүй.
- Destructive үйлдэл (устгах) — улаан, гэхдээ primary улаан товч ганц алхамд шууд устгахгүй (урсгал: 11-dashboard-patterns.md → Destructive үйлдэл).
- Icon-only товч: 36×36 (md), icon 16px, заавал `aria-label` + tooltip.

## Форм

### Анатоми

- **Label заавал, дээр нь** — placeholder-ийг label болгон хэрэглэхгүй (бичиж эхлэхээр алга болж контекст алдагдана).
- Input өндөр товчтой ижил (md 36 / lg 40); mobile-д font-size ≥16px (iOS zoom-ээс сэргийлнэ), desktop-д 14px болно.
- Ганц баганаар — хоёр багана форм бөглөх дарааллыг эвддэг (нэр/овог мэтийн жижиг хос л зэрэгцэж болно). Форм хуудасны max-width 720px.
- Required-ийг `*`-ээр биш, харин цөөн optional талбараа «(заавал биш)» гэж тэмдэглэ.
- Helper текст label-ийн доор биш, input-ийн доор (алдааны текст түүнийг орлоно, байрлал үсрэхгүй).
- Autocomplete attribute-уудыг өг (`autocomplete="email"` гэх мэт) — UX + password manager.

### Validation — хэзээ, хаана

| Мөч | Үйлдэл |
|---|---|
| Анхны бичилт | Шалгахгүй (эхний keystroke дээр алдаа хэзээ ч биш) |
| Эхний blur (талбар хүрсний дараа) | Тухайн талбарыг шалгаж алдааг харуул |
| Алдаатай болсны дараа | `input` бүрт дахин шалга — засмагц алдаа алга болно |
| IME/compose (кирилл гар) | `compositionend` хүлээ, дундуур нь шалгахгүй (09-localization-mn.md) |
| Submit | Бүх талбар; алдаа >5 бол дээр нь **error summary** (`role="alert"`, алдаа бүр талбар руу anchor) + эхний алдаатай талбарт фокус |
| Сервер алдаа (409 давхардал г.м.) | Холбогдох талбарын доор; талбаргүй бол формын дээд хэсэгт |

- Алдаа **талбарын доор, `--danger-foreground` текст + icon**, `aria-describedby`-гаар холбогдсон, `aria-invalid="true"`.
- Алдааны өгүүлбэрийн томьёо: юу буруу + яаж засах («Имэйл хаяг `@` агуулах ёстой») — 10-ux-writing-mn.md.
- Амжилт төлөв талбар бүрт биш — зөвхөн нууц үгийн хүч, username available мэт async шалгалтад (✓ + текст).

### Input төрөл / inputmode матриц

| Өгөгдөл | `type` | `inputmode` | `autocomplete` | Тэмдэглэл |
|---|---|---|---|---|
| Имэйл | `email` | — | `email` | `autocapitalize="off"`, `spellcheck="false"` |
| Утас | `tel` | `tel` | `tel` | E.164 хадгал, `+976 XXXX XXXX` харуул |
| Бүхэл тоо (тоо ширхэг, дүн) | `text` | `numeric` | — | `type="number"`-ийн scroll/`e` асуудалгүй; өөрөө parse |
| Бутархай | `text` | `decimal` | — | `.` тусгаарлагч |
| Нууц үг | `password` | — | `current-password` / `new-password` | paste хориглохгүй (WCAG 3.3.8) |
| OTP код | `text` | `numeric` | `one-time-code` | доор үз |
| Хайлт | `search` | — | — | доор «Search» |
| URL | `url` | `url` | `url` | `autocapitalize="off"` |
| Огноо | `date` / `datetime-local` | — | — | native эхэлж (Date/Time хэсэг) |
| Username / ID | `text` | — | `username` | `autocapitalize="off"`, латин-ыг шалга (09) |

### Тусгай талбарууд

- **Нууц үг show/hide**: баруун талд icon-товч (`aria-label="Нууц үг харуулах"` / «нуух», `aria-pressed`), 24×24 hit target; toggle дарахад фокус input-д үлдэнэ; `type` солих нь autofill-ийг эвдэхгүй. Шаардлага (≥8 тэмдэгт г.м.) талбарын дээр биш доор, checklist хэлбэрээр, бичих тусам ✓.
- **OTP**: нэг `<input inputmode="numeric" autocomplete="one-time-code" maxlength="6">` давуу (paste, SMS autofill ажиллана); 6 тусдаа нүд хийвэл paste-ийг бүгдэд тараах логик заавал, Backspace өмнөх нүд рүү. Дахин илгээх товч 30–60с countdown-тэй, хугацаа `aria-live="polite"`.
- **Mask** (утас, карт, регистр): бичих явцад форматла, гэхдээ хадгалах утга цэвэр (`+976XXXXXXXX`); курсорын байрлалыг хадгал; placeholder-д жишээ формат (`УБ12345678`). Mask-аар paste-ийг бүү хааж.
- **Тэмдэгтийн тоолуур**: `max-length` байгаа talbarт «120/280» баруун доор, 90%-д muted → warning, хэтэрвэл danger + submit хориглоно; `aria-live="polite"` (хэтэрсэн үед л зарлана, үсэг бүрт биш). Хатуу `maxlength` attribute-аар тасалж бичүүлэхгүй — paste-д утга алдагдана.
- **Autosave** (draft): 1–2с debounce, хадгалсан/хадгалж байна төлөв формын доод/дээд нэг газар inline («Хадгалагдлаа · 14:32»), toast биш; алдвал retry товчтой inline error; localStorage-д draft хадгалах бол хуудас ачаалахад «Үргэлжлүүлэх» санал. Autosave-тэй форм дээр «Хадгалах» товч байхгүй (хоёр горим хольдоггүй — 11-dashboard-patterns.md → Settings).
- **Disabled vs read-only**: *disabled* = энэ контекстэд ажиллахгүй (эрхгүй, урьдчилсан нөхцөл дутуу) — яагаад гэдгийг tooltip/helper-ээр; фокус авахгүй, form-д илгээгдэхгүй. *read-only* = утга мэдээлэл, засахгүй — фокус авна, copy хийгдэнэ, `readonly` attribute, харагдац text шиг (border-гүй/бүдэг). Зөвхөн харуулах зорилгоор disabled хэрэглэхгүй (contrast унаж уншигдахгүй).
- **Select vs radio vs checkbox** — доорх тусдаа хэсэг.

## Table (data-нягт UI)

- Тоон багана **баруун зэрэгцээ + tabular-nums** (`font-variant-numeric: tabular-nums`), текст зүүн.
- Мөрийн өндөр: **compact 36 / default 44 / comfortable 52**.
- Zebra striping ЭСВЭЛ row border — хоёуланг нь биш.
- Header: 12–13px, 500 weight, muted өнгө; uppercase бол `letter-spacing: 0.06em` (кирилл), ерөнхийдөө sentence case.
- Урт table-д sticky header; mobile-д хэвтээ scroll (`overflow-x: auto`) эсвэл card болгон эвхэх.
- Хоосон нүд: `—` (em dash), 0 болон хоосныг ялга.
- Sort / filter / selection / pagination / row action зан төлөв → 11-dashboard-patterns.md → Table.

## Card

- Padding: **16px (compact, mobile) / 24px (default desktop)**. 20 хэзээ ч биш.
- Radius `--radius-lg` (8px); border 1px, shadow-гүй (хөвөгч биш).
- Нэг card = нэг ойлголт; card дотор card-аас зайлсхий.
- Бүхэлдээ clickable бол hover төлөвтэй + доторх линкүүд nested interactive болохоос сэргийл.

## Компонентын 5 төлөв

Өгөгдөл ачаалдаг компонент бүр **5 төлөвтэй** гэж төлөвлө — нэг загвар, бүх файлд ижил:

| Төлөв | Дүрэм |
|---|---|
| **loading** | Layout-тай ижил хэлбэрийн skeleton; 300ms хойшлуулж, гарсан бол ≥500ms (11-dashboard-patterns.md → Feedback) |
| **empty** | Хоёр дэд төрөл: *first-run* (юу болох + primary CTA) ба *filtered* (шүүлтүүрийн үр дүн 0 → «Цэвэрлэх»); хүснэгт 11-dashboard-patterns.md → Хоосон төлөв |
| **error** | Юу болсон + юу хийх (retry товч); техник код хэрэглэгчид ил гаргахгүй |
| **success** | Бодит контент; optimistic UI зөвхөн хурдан, буцаагдах үйлдэлд |
| **permission-denied** | Хоосон хуудас биш: «эрх хүрэхгүй» + хэн өгөх + хүсэлт/буцах зам; нуух/disable/тайлбарлах дүрэм 11-dashboard-patterns.md |

## Modal / Dialog

- Өргөн: alert 400px / форм 480–560px / том контент 720px+. Full-screen нь mobile-д.
- Focus trap + Esc хаана + overlay дарахад хаагдана (форм бөглөж байсан бол баталгаажуул); хаагдахад фокус trigger рүү буцна.
- `<dialog>.showModal()` эсвэл `inert` — арын контент screen reader-ээс ч нуугдана. Давхарга/scroll-lock дүрэм 14-app-resilience.md.
- Modal доторх modal — дизайны алдааны шинж; nested хэрэгтэй бол flow-гоо эргэнэ хар.
- Радиус `--radius-xl`, shadow `--shadow-xl`, duration 240ms.

## Toast / Notification

- Байрлал нэг л газар (ихэвчлэн баруун дээд/доод), `--z-toast`.
- Success 3–5 сек өөрөө алга болно; error нь хэрэглэгч хаатал үлдэнэ; hover дээр timer зогсоно.
- Үйлдэлтэй toast (Undo): **5с default, bulk destructive-д 10с** — урсгал 11-dashboard-patterns.md → Destructive үйлдэл.
- `role="status"` (success) / `role="alert"` (error) live region хуудас ачаалахад DOM-д байх (07-accessibility.md).

## Tabs

- **Нэг мөрөнд ≤6 tab** — илүү бол dropdown/sidebar nav руу шилж.
- Идэвхтэй tab: 2px underline indicator + текст өнгө контраст; зөвхөн өнгөөр ялгахгүй (07-accessibility.md-г үз).
- Tab өндөр 40px, хоорондын зай 16–24px; label 1–2 үг (`white-space: nowrap; min-width`), icon бол заавал текст-тэй.
- Keyboard: `←`/`→` tab солино, `Tab` нь panel руу орно; `role="tablist"` / `role="tab"` / `aria-selected`.
- Идэвхтэй tab-ыг URL-тай sync хий (`?tab=billing`) — refresh, share хийхэд хадгалагдана.
- Tab дотор tab бүү үүрлүүл — хоёр дахь түвшин нь segmented control эсвэл sub-nav.
- Tab солиход scroll position үсрэхгүй байх: panel-ийн min-height тогтоо.

## Dropdown / Menu

- **≤8 item**; илүү бол дээр нь хайлтын талбар нэм (combobox болгоно).
- Мөрийн өндөр 36px, хэвтээ padding 12px, panel өргөн 180–280px, radius `--radius-lg`, `--shadow-md`, `--z-dropdown`.
- Логик бүлэгт хуваахдаа 1px divider + 4–8px зай; бүлэг бүр ≤4–5 item.
- Destructive үйлдэл (Устгах) хамгийн сүүлд, divider-ээр тусгаарлаад улаан текст.
- Icon хэрэглэвэл бүх item-д, эсвэл нэгэнд ч биш. Icon 16px, текстээс 8px зайтай.
- Keyboard: `↑`/`↓` явна, `Enter` сонгоно, `Esc` хаана, эхний үсгээр үсэрнэ; фокус эхлээд эхний item дээр.
- Trigger товчин дээр `aria-haspopup="menu"` + `aria-expanded`; viewport-оос гарах бол автоматаар дээш/хажуу тийш эргэнэ.
- Одоогийн сонголтыг checkmark-аар тэмдэглэ (select-маягийн menu).

## Tooltip vs Popover

| | Tooltip | Popover |
|---|---|---|
| Нээх | hover + keyboard focus | click / tap |
| Агуулга | зөвхөн текст, ≤1 мөр (~60 тэмдэгт) | гарчиг, текст, товч, форм ч болно |
| Хаах | hover/focus алдагдахад | гадна дарах, `Esc`, хаах товч |
| Mobile | байхгүй (hover үгүй) | ажиллана |
| z-index | `--z-tooltip` 1600 | `--z-popover` 1400 |

- Tooltip нээгдэх хоцрогдол **500ms**, хаагдах 0–100ms; нэг tooltip нээлттэй үед дараагийнх нь хоцрогдолгүй. Chart tooltip ≤150ms (12-data-viz.md).
- Tooltip-д тавьсан мэдээлэл **зайлшгүй бол болохгүй** — зөвхөн нэмэлт тайлбар; чухал мэдээлэл харагдах текст байна.
- Icon-only товч бүр tooltip + `aria-label` хоёулантай.
- Tooltip дотор interactive элемент тавихгүй; хэрэгтэй бол popover.
- Popover өргөн 240–360px, padding 16px, заагч сум (arrow) заавал биш; нээгдэхэд фокус дотор нь орно, хаахад trigger руу буцна.
- Mobile-д tooltip-ийн оронд: харагдах helper text, эсвэл `(i)` товч → popover/bottom sheet.

## Badge / Status

- **Статус = icon + текст**, зөвхөн өнгөт цэг биш (өнгө ялгадаггүй хэрэглэгч + хэвлэхэд).
- Semantic өнгө дээд тал нь 5: neutral / info / success / warning / danger. 6 дахь «статус» гарвал өнгө биш текстээр ялга.
- Хэмжээ: өндөр 20–24px, текст 12px (доод хязгаар), хэвтээ padding 6–8px, 500 weight.
- Өнгөт фон: `--{status}-subtle` + `--{status}-foreground` текст (08-design-tokens.md) — контраст ≥4.5:1.
- Sentence case давуу; uppercase хэрэглэвэл зөвхөн 12px-д, `letter-spacing: 0.06em` (кирилл).
- Тооны badge (notification count): pill (`--radius-full`), min-width 18–20px, 99-өөс дээш бол «99+».
- Presence (online/offline): 8–10px цэг, аватарын баруун доод буланд, 2px фонтой ижил өнгийн ring.
- Badge-ийг товч биш — дарагддаг бол tag/chip эсвэл filter.

## Avatar

- Хэмжээ тогтмол 3–4 шат: **24 / 32 / 40px** (+ 64–96px profile хуудсанд); тойрог хэрэглэгчид, квадрат (`--radius-md`) байгууллага/workspace-д.
- Зураггүй бол initials fallback: нэр, овгийн эхний үсэг (Кириллд ч мөн адил — «Батболд Сүхбат» → «БС»), 1–2 үсэг, uppercase, хэмжээний 40–45% font-size.
- Initials-ийн фон: нэрний hash-аас тогтмол сонгосон 6–8 зөөлөн өнгө — refresh бүрт солигддоггүй.
- Зураг: `object-fit: cover`, `alt`-д нэр (зөвхөн чимэглэл бол `alt=""`).
- Статус ring: `box-shadow: 0 0 0 2px var(--background), 0 0 0 4px var(--ring)`.
- Avatar group: давхцал −8px (32px) / −6px (24px), **≤4 харуулаад «+N»**; давхцсан хэсэг 2px фон өнгийн border-той.
- Ачаалж байхад initials-ийг шууд харуул (skeleton биш) — зураг ирэхэд солино.

## Tag / Chip

- Өндөр 24–32px, текст 12–13px, padding 4–8px / 8–12px, radius `--radius-md` эсвэл pill.
- Устгах `×` нь **hit target ≥24×24px** (харагдах icon 12–14px байсан ч) + `aria-label="Устгах: {нэр}"`.
- Гурван төрлийг хольж болохгүй:
  - **Input chip** — хэрэглэгчийн оруулсан утга (email, tag); устгагдана.
  - **Filter chip** — toggle, сонгогдвол checkmark + фон; олон сонгож болно.
  - **Choice chip** — radio шиг, нэгийг л сонгоно.
- Олон chip-ийг **wrap** хий, хэвтээ scroll биш (scroll нь далд chip-үүдийг нуудаг; mobile-д л 1 мөр scroll болно).
- Chip-ийн урт текст: max-width 160–200px + `overflow-wrap: anywhere`/ellipsis, бүтнийг title/tooltip-оор (09-localization-mn.md overflow хүснэгт).
- Input chip талбарт `Backspace` сүүлийн chip-ийг сонгож, дахин дарахад устгана; `Enter`/`,` шинэ chip үүсгэнэ.
- Сонгогдсон filter chip-үүдийн тоог «Шүүлтүүр (3)» гэж харуул + «Цэвэрлэх» нэг товч.

## Select vs Radio vs Checkbox

| Сонголтын тоо | Нэгийг сонгох | Олныг сонгох |
|---|---|---|
| 2–5 | radio (бүгд харагдана) | checkbox |
| 6–15 | `<select>` | multi-select combobox |
| 15+ | searchable combobox | searchable multi-combobox + сонгосон chip |

- **≤5 сонголт бол radio/checkbox** — нэг дарахад харагдана, select нь нэмэлт click.
- Boolean: үр дүн нь **шууд** (дарангуут хадгалагдана) бол switch; форм submit-ээр хадгалагддаг бол checkbox.
- Radio-д default сонголт заавал байх (эсвэл ухамсартайгаар «Сонгоогүй»); checkbox-д default нь ихэвчлэн unchecked.
- Radio/checkbox-ийн hit target: бүх label дарагдана, мөрийн өндөр ≥32px, icon 16–20px.
- Switch-ийн label нь төлөв биш, тохиргооны нэр: «Мэдэгдэл» (On/Off биш «Идэвхтэй/Идэвхгүй» гэж label өөрчлөхгүй).
- Native `<select>` mobile-д хамгийн сайн — custom select зөвхөн хайлт, icon, бүлэглэл хэрэгтэй үед.
- Сонголтын тоо тодорхойгүй (API-аас ирдэг) бол босгоор компонент сол: `n <= 5 ? radio : n <= 15 ? select : combobox`.

## Date / Time

- Харуулах: **≤7 хоногийн доторх бол харьцангуй** («3 минутын өмнө», «өчигдөр 14:20»), түүнээс хуучин бол абсолют.
- Харьцангуй цаг үргэлж `title`/tooltip-оор абсолют утгатай: `<time datetime="2026-08-20T14:20:00+08:00" title="2026-08-20 14:20">`.
- Абсолют формат **нэг л**: `yyyy-MM-dd HH:mm` (24 цаг), зөвхөн огноо `yyyy-MM-dd`; жил орхихгүй, «20 авг» хувилбаргүй (09-localization-mn.md).
- Хүснэгт, лог, тайланд харьцангуй биш абсолют — эрэмбэлэх, харьцуулахад хэрэгтэй.
- Харьцангуй цагийг 60 сек тутам шинэчил, «0 секундын өмнө» биш «дөнгөж сая».
- Picker: эхлээд **native `<input type="date">` / `type="datetime-local"`** (mobile keyboard, locale, хүртээмж бэлэн); custom picker зөвхөн range, preset («Сүүлийн 7 хоног»), disabled өдрүүд хэрэгтэй үед. Долоо хоног Даваагаас.
- Custom range picker: 2 сар зэрэгцээ (desktop) / 1 сар (mobile), preset жагсаалт зүүн талд, сонгосон муж фон өнгөөр.
- Гараар бичих боломж үргэлж үлдээ (picker дангаараа биш) + format placeholder «2026-08-20».
- Timezone: хадгалахдаа UTC, харуулахдаа Asia/Ulaanbaatar (UTC+8); олон бүсийн хэрэглэгчтэй бол товчлол нэм («14:20 ULAT») — дэлгэрэнгүйг 09-localization-mn.md-г үз.

## File upload

- Drag-drop зон + **«Файл сонгох» товч хоёулаа** (drag нь mobile, keyboard-д байхгүй); зон бүхэлдээ дарагдана.
- Зөвшөөрөх төрөл, дээд хэмжээ, тоог **урьдчилан бич**: «PNG, JPG, PDF · нэг файл ≤10MB · хамгийн ихдээ 5»; `accept` attribute-аар давхар хязгаарла.
- Файл бүрт өөрийн мөр: нэр (ellipsis), хэмжээ, progress bar (0–100%), cancel `×`.
- Алдаа файл тус бүрээр (нэг файл унавал бусад нь үргэлжилнэ): «too-big.pdf — 10MB-аас хэтэрсэн (14MB)».
- Зургийн preview thumbnail 48–64px, квадрат `object-fit: cover`; бусад төрөлд file-type icon.
- Upload амжилттай бол устгах/солих товч; форм submit хүртэл upload-ыг хойшлуулахгүй (сонгонгуут эхэлнэ) — гэхдээ submit-ээс өмнө бүгд дууссан эсэхийг шалга.
- Drag зон хэмжээ: өндөр ≥120px, dashed 2px border, drag-over төлөвт фон + border өнгө солигдоно.
- Урт upload (>10 сек) — хуудас солиход анхааруулга (`beforeunload`).

## Search

Анатоми энд; dashboard дахь зан төлөв (debounce, URL, үр дүнгийн тоо, хоосон үр дүн) → 11-dashboard-patterns.md → Search.

- `<input type="search">` — native семантик хангалттай, нэмэлт `role` тавихгүй; `<form role="search">` эсвэл `<search>` элемент хүрээлнэ.
- Icon зүүн, clear `×` баруун (утгатай үед л, hit target 24×24).
- Loading: талбарт жижиг spinner, өмнөх үр дүнг арилгахгүй (layout үсрэхгүй).
- Autocomplete dropdown: ≤8 санал, `↑`/`↓` + `Enter`, сүүлийн хайлтууд эхэнд; `role="combobox"` + `aria-expanded` + `aria-controls` (APG Combobox).

## Multi-step форм

- **≤5 алхам**; илүү бол алхмуудыг нэгтгэ эсвэл хоёр тусдаа flow болго.
- Stepper: дугаар + label («1 Мэдээлэл · 2 Хаяг · 3 Төлбөр»), одоогийн алхам 600, дууссан нь checkmark; mobile-д «2/4 · Хаяг» гэсэн товч хувилбар.
- Алхам бүрийг **тухайн алхамд нь** validate хий, дараагийн руу шилжихээс өмнө; бүх алдааг сүүлд овоолохгүй.
- «Буцах» үргэлж боломжтой, оруулсан утга хадгалагдана; дууссан алхам руу stepper-ээс шууд очиж болно (дараагийнх руу биш).
- Draft автоматаар хадгалагдана (дээрх Autosave) — хуудас хаагаад буцаж ирэхэд «Үргэлжлүүлэх» сонголт.
- Submit-ийн өмнө **summary алхам**: бүх утгыг алхам тус бүрээр харуулаад «Засах» линктэй.
- Товчнууд: баруун «Үргэлжлүүлэх» (primary), зүүн «Буцах» (secondary); сүүлд «Илгээх»; `Enter` = үргэлжлүүлэх.
- Алхам бүр 3–7 талбар; нэг алхамд 1–2 талбар бол нэгтгэ.
- Progress %-ийг ойролцоогоор биш алхмаар хэмж — «60%» биш «3/5».

## Onboarding / first-run

- **Empty state өөрөө onboarding**: хоосон дэлгэц бүр юу хийхийг заана + нэг primary CTA (дээрх 5 төлөв); тусдаа tour ихэвчлэн хэрэггүй.
- Checklist ≤3–5 алхам («Профайл бөглөх · Эхний төсөл үүсгэх · Гишүүн урих»), гүйцэтгэсэн нь checkmark, прогресс «2/3»; dashboard-ын булан/sidebar-д, modal биш.
- Хаах боломжтой (dismiss) + буцааж нээх зам (Тусламж цэсэнд); хаасныг серверт хадгал — дахин гарч ирэхгүй.
- Modal tour хийх бол **≤3 алхам**, алгасах товч алхам бүрт, нэг л удаа; 4+ алхамтай tour-ыг хэн ч дуусгадаггүй.
- Tooltip-маягийн заавар (coachmark) нэг удаад 1 л; тухайн feature-ийг анх харахад, эхний минутад бүгдийг биш.
- Эхний утга автоматаар бэлд (sample data, default төсөл) — хоосон биш «бэлэн» мэдрэмж; sample-ийг тодорхой тэмдэглээд нэг товчоор устгана.
- Заавал бөглөх зүйлийг бүртгэлийн дараа биш, хэрэглэх мөчид асуу (progressive): төлбөрийн мэдээллийг эхний төлбөрт.
- «Шинэ» badge нь 7–14 хоног л, дараа нь алга болно.

## 11-dashboard-patterns.md-д эзэмшигдсэн сэдвүүд

Дараах сэдвүүд өмнө нь энд давхардаж байсан; одоо **зөвхөн** [11-dashboard-patterns.md](11-dashboard-patterns.md)-д:

- Confirmation хэв маяг (Undo toast / modal confirm / type-to-confirm) → «Destructive үйлдэл ба баталгаажуулалт»
- Pagination (offset / cursor / infinite, page size, URL) → «Pagination»
- Search-ийн зан төлөв (debounce, `/` shortcut, URL `?q=`, 0 үр дүн) → «Search»
- Command palette, keyboard shortcut → «Keyboard ба command palette»

## Эх сурвалж

- Apple Human Interface Guidelines — Components: Buttons, Text fields, Toggles, Pickers, Menus, Popovers, Alerts, Tab bars, Segmented controls, Progress indicators — developer.apple.com/design/human-interface-guidelines/components
- Material Design 3 — Components: Buttons, Text fields, Tabs, Menus, Tooltips, Badges, Chips, Checkbox, Radio button, Switch, Date pickers, Dialogs, Snackbar, Search — m3.material.io/components
- W3C WAI-ARIA Authoring Practices Guide (APG) — Patterns: Tabs, Menu and Menubar, Menu Button, Tooltip, Dialog (Modal), Combobox, Radio Group, Switch, Listbox, Grid — w3.org/WAI/ARIA/apg/patterns/
- WCAG 2.2 — 1.4.1 Use of Color, 1.4.3 Contrast (Minimum), 2.1.1 Keyboard, 2.4.3 Focus Order, 2.5.8 Target Size (Minimum), 3.3.1 Error Identification, 3.3.3 Error Suggestion, 3.3.8 Accessible Authentication, 4.1.2 Name, Role, Value — w3.org/TR/WCAG22/
- Nielsen Norman Group — «Tabs, Used Right»; «Drop-Down Menus: Use Sparingly»; «Tooltip Guidelines»; «Checkboxes vs. Radio Buttons»; «Toggle-Switch Guidelines»; «Date-Input Form Fields»; «Onboarding Tutorials vs. Contextual Help»; «Empty State»; «How to Report Errors in Forms»; «Password Creation: 3 Ways to Make It Easier» — nngroup.com/articles/
- GOV.UK Design System — Components: Text input, Password input, Character count, Error summary, Error message, Date input, File upload, Radios, Checkboxes, Select — design-system.service.gov.uk
- Refactoring UI (Adam Wathan, Steve Schoger) — «Hierarchy is Everything», «Designing Text», «Working with Color», «Finishing Touches» — refactoringui.com
- MDN Web Docs — `<input>` types, `inputmode`, `autocomplete`, `readonly`/`disabled`, `<input type="search">`, `<search>`, `<time>`, `<select>`, `<dialog>`, Popover API — developer.mozilla.org
- web.dev — «Building a tooltip component», «Building a dialog component», «Building a tabs component»; «Sign-in form best practices»; «SMS OTP form best practices» — web.dev
- Shopify Polaris — Components: Tabs, Popover, Badge, Avatar, Tag, DropZone — polaris.shopify.com/components
