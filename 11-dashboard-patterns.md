[← Индекс руу буцах](README.md)

# Dashboard / SaaS / ERP / admin UI-ийн бүтцийн хэв маяг

## Яагаад

Админ, ERP, SaaS апп-ыг хэрэглэгч өдөр бүр олон цагаар хэрэглэнэ. Landing page шиг «сэтгэгдэл төрүүлэх» биш, **урьдчилан таамаглаж болох, хурдан, алдаанаас хамгаалсан** байх нь гол. Доорх хэв маягууд нь Linear, Stripe Dashboard, GitHub, Notion, Atlassian, Material 3, Carbon зэрэг системүүдэд давтагддаг нийтлэг шийдлүүд. Компонентын **анатоми** (товч, форм, table, modal, toast-ын хэмжээ/төлөв) 06-components.md хариуцна — энд **бүтэц, урсгал, зан төлөв**: хайлт, pagination, баталгаажуулалт, keyboard, permission, хадгалалт. Энэ файл өөрийн домэйнд 01–08-ыг дарна.

## App shell

| Хэсэг | Хэмжээ | Тайлбар |
|---|---|---|
| Sidebar (дэлгэсэн) | 240–280px (default 240) | label + icon |
| Sidebar (хумьсан) | 56–64px (default 56) | зөвхөн icon, hover-т tooltip (label-гүй icon хориотой) |
| Top bar | 48–64px (default 56) | breadcrumb/хайлт/tenant/профайл; `position: sticky`, `--z-sticky` |
| Content padding | 24–32px (desktop) / 16px (mobile) | 03-spacing-layout.md-г үз |
| Content max-width | **fluid, ≤1536px** (table, chart, overview) · **720px** (форм, settings, текст) | хуудасны төрлөөр; нэг хуудсанд хольж болно |

- **Sidebar ≤1024px-д drawer болно** (overlay + focus trap + Esc); 1024–1280px-д хумьсан горимоор эхлүүл.
- Доод хэсэг (settings/profile) `position: sticky; bottom: 0`; sidebar фон `--background-subtle` (content-оос нэг шат).
- Mobile bottom tab bar, PWA — 14-app-resilience.md.

## Тохиргоо хаана хадгалагдах

| Төрөл | Хаана | Жишээ |
|---|---|---|
| Төхөөрөмжийн харагдац (density prefs) | `localStorage` | sidebar хумьсан/дэлгэсэн, density compact/default, theme |
| Өгөгдлийн харагдац (data prefs) | **URL query** | шүүлтүүр, сорт, хуудас, page size, tab, хайлтын `q`, огнооны муж |
| Аккаунтын тохиргоо | Сервер (хэрэглэгчийн мөр) | хэл, timezone, мэдэгдлийн тохиргоо, onboarding дууссан эсэх |

- URL-д хадгалагдах зүйл localStorage-д давхар хадгалагдахгүй — хоёр эх сурвалж зөрдөг. Refresh, back, share гурвууланд URL л ажиллана.
- Хэлний сонголт — 09-localization-mn.md (public → URL, апп → сервер + cookie).

## Navigation

- **Дээд түвшний цэс ≤7**; хэтэрвэл бүлэглэ (group label 12px muted, sentence case; бүлэг хооронд 16–24px).
- Active төлөв = **accent bar (зүүн талд 2–3px) + font-weight 500 + `--accent-subtle` background** — зөвхөн өнгөөр тэмдэглэхгүй (07-accessibility.md, WCAG 1.4.1); `aria-current="page"`.
- Гүн ≥3 (Захиалга → #1042 → Засах) бол **breadcrumbs** заавал; 2 түвшинд «← Буцах» хангалттай. Сүүлийн элемент линк биш. Хуудас доторх tab `h1`-ийн доор, URL-тай.
- Хуудас бүр: **`h1` = хуудасны нэр (зүүн), primary action = баруун дээд**, нэг мөрөнд. Primary товч нэг л байна.
- SPA route солигдоход `<title>`, фокус, screen reader зарлал — 14-app-resilience.md.

## Хуудасны загварууд

### Overview (нүүр)

- Дараалал: **KPI мөр → chart → table**. KPI 3–4 ширхэг (6-аас олонгүй); tile-ийн анатоми 12-data-viz.md.
- Хугацааны шүүлтүүр (7d/30d/90d) дээд талд нэг газар, бүх widget-д нийтлэг, URL-д; table 5–10 мөр + «Бүгдийг үзэх».

### List → Detail

| Хэлбэр | Хэзээ |
|---|---|
| **Тусдаа route** (`/orders/1042`) | Detail нь 1 дэлгэцээс урт, засвартай, хуваалцах URL хэрэгтэй — default |
| **Master-detail** (зүүн list 320–400px + баруун panel) | Мөр хооронд хурдан шилжих (мэйл, чат, тикет); ≥1280px-д л, URL-тай, доош route болно |
| **Side sheet / drawer** (480–640px) | Унших + 1–3 талбар засах; list-ийн контекст алдагдахгүй |

- Detail-ээс буцахад list-ийн scroll байрлал, шүүлтүүр хадгалагдсан байна (URL query-д хадгал).

### Settings

- Зүүн талд босоо tab (200–240px), баруун талд хэсгүүд; **хэсэг бүрийн max-width 720px** — форм бүтэн өргөнөөр сунахгүй.
- Хадгалах хоёр горим, хольж болохгүй:
  - **Autosave** — toggle, select, ганц талбар; «Хадгалагдлаа» 2 сек (toast биш, inline текст). Дэлгэрэнгүй 06-components.md → Форм → Autosave.
  - **Явцуу «Хадгалах»** — олон талбартай форм; товч **sticky footer**-т (56–64px), өөрчлөлт байхгүй үед disabled, «Болих» хажууд.

### Auth хуудсууд

- Нэг багана, card өргөн 400–440px, дэлгэцийн төвд; лого дээр, «Бүртгүүлэх/Нэвтрэх» шилжилт доор.
- Талбар ≤3 (email, нууц үг, [remember]); SSO товчнууд дээр, `— эсвэл —` divider-тэй. Алдаа «Имэйл эсвэл нууц үг буруу» — аль нь гэдгийг хэлэхгүй. Паст хориглохгүй, password manager ажиллана (WCAG 3.3.8).
- Session дуусах анхааруулга — 14-app-resilience.md.

## Density (нягтралын горим)

| Горим | Table мөр | Товч/input | Body font | Хэрэглээ |
|---|---|---|---|---|
| Compact | 36px | sm 32px | 13px | ERP, санхүү, олон мөр |
| Default | 44px | md 36px (lg 40 primary) | 14px | ихэнх SaaS |
| Comfortable | 52px | lg 40px | 14px | унших чиглэлтэй, touch-д |

- Хэрэглэгчид **toggle өг, сонголтыг `localStorage`-д** хадгал (дээрх хүснэгт — төхөөрөмжийн харагдац).
- Compact горимд ч pointer target ≥24×24px (WCAG 2.5.8); mobile-д compact санал болгохгүй.

## Table — зан төлөв

Анатоми (зэрэгцүүлэлт, zebra, header, мөрийн өндөр, sticky) 06-components.md-д. Энд үйлдлүүд.

- **Sort**: сум зөвхөн идэвхтэй багана дээр харагдана; бусад багананд hover үед muted сум. Header дарахад asc → desc → default гэсэн 3 шат. `?sort=created&dir=desc`.
- **Filter**: table-ийн дээр chip хэлбэрээр (`Төлөв: Идэвхтэй ×`); «Шүүлтүүр» товч дээр **идэвхтэй тоо badge** (`Шүүлтүүр (3)`); ≥2 шүүлтүүр үед «Бүгдийг цэвэрлэх». URL query-д; шүүлтүүр өөрчлөгдвөл `page=1`.
- **Selection + bulk bar**: checkbox багана 40–48px; ≥1 сонгоход header-ийн оронд **sticky bulk bar** гарна: «3 сонгосон · [Экспорт] [Архивлах] [Устгах] · Бүгдийг сонгох (1,284)». Destructive bulk үйлдэл confirm + Undo 10с. «Бүх хуудсыг сонгох» нь тусдаа, тодорхой линк.
- **Row action**: inline icon ≤2 (засах, үзэх), бусад нь `⋯` overflow menu-д; destructive нь menu-ийн хамгийн доор, divider-ийн дараа, улаан. Мөрийг бүхэлд нь clickable болговол action товчнууд `stopPropagation`.
- **Mobile**: эхний багана (нэр/ID) `position: sticky; left: 0` + баруун талд сүүдэр; ≤3 чухал багана үлдээж, бусдыг detail рүү.
- Truncation: `white-space: nowrap; overflow: hidden; text-overflow: ellipsis` + `title` attribute; багана `min-width`-тэй (кирилл 120–160px), `max-width` 240–320px.
- Тоо, огноо, мөнгө — баруун, `tabular-nums`; огноо **`yyyy-MM-dd HH:mm`** л, relative («3 цагийн өмнө») бол `title`-д абсолют.
- **Status багана = badge: icon + текст**, өнгө ганцаараа биш; төлөвийн нэр ≤2 үг; статусын өнгө ≤5.

## Search

- Анатоми (`type="search"`, icon, clear, autocomplete) 06-components.md.
- Бичихэд хайдаг бол **debounce 300ms** + хамгийн сүүлийн хүсэлт л үр дүн өгнө (`AbortController`, race-ээс сэргийл); ≥2 тэмдэгтээс эхэлнэ.
- `/` товч фокус авчирна (текст талбарт байхгүй үед), `Esc` цэвэрлээд фокус гаргана.
- Үр дүнгийн тоо ил: «128 илэрц · «батболд»»; 0 бол юу бичсэнийг давтаад **санал** өг («Зөв бичгийг шалга», ойролцоо хайлт, шүүлтүүр арилгах) — filtered empty state.
- Таарсан хэсгийг `<mark>`-аар тодруул (bold, өнгөт фон биш); Кирилл/латин, том/жижиг үсэг ялгахгүй (`localeCompare` / `normalize`).
- Хайлт хийхэд URL шинэчил (`?q=`) — back, share ажиллана; хуудас солиход утга хадгалагдана.
- Хайж байхад spinner input дотор, table-ийг skeleton болгохгүй, өмнөх үр дүн үлдэнэ.

## Pagination

| Хэлбэр | Хэзээ |
|---|---|
| **Offset** (1 … 4 5 6 … 40) — default | ≤10k мөр, хэрэглэгч тодорхой хуудас руу үсрэх хэрэгтэй |
| Cursor («Өмнөх / Дараах», «Цааш үзэх») | >10k мөр эсвэл амьд өөрчлөгддөг өгөгдөл — offset мөр давхцуулна/алгасна; нийт тоо ойролцоогоор («1,000+») |
| Infinite scroll | **зөвхөн feed/лог**; table-д хэзээ ч биш (footer хүрэхгүй, байрлал алдагдана); дотор нь ч «Цааш үзэх» fallback + scroll position сэргээх |

- Offset: ≤7 товч харагдана, одоогийн нь тод, `aria-current="page"`; prev/next icon + текст.
- Байршил, тоо ил: «1–25 / 1,240»; нийт тоолох удаан бол «1–25 / 1,000+».
- Page size **25 / 50 / 100**, default 25 (compact-д 50); `?page=3&size=50` URL-д — back, refresh, share ажиллана.
- Хуудас солиход жагсаалтын эхэнд scroll, фокус table-ийн caption/гарчиг руу.
- Mobile-д дугааруудыг prev/next + «3 / 40» болгон цөөл.
- Table-д pagination доор: сонголт/нийт мөрийн тоо зүүн, дугаар баруун.

## Detail хуудас

- **Header блок**: breadcrumb → `h1` (нэр/ID) + status badge нэг мөрөнд → meta мөр (үүсгэсэн, эзэмшигч, сүүлд зассан; 13px muted, `·`-ээр тусгаарла) → actions баруун талд (primary 1 + secondary 1–2 + `⋯`). Header `position: sticky; top: 0`.
- Хэсэг ≤3 бол нэг хуудсанд дараалуулж (anchor link-тэй); **>3 бол tab**. Tab-ын нэр 1–2 үг, тоотой байж болно («Төлбөр (4)»).

## Апп доторх форм

| Талбарын тоо | Хэлбэр |
|---|---|
| 1 | Inline edit (дарахад input болно, Enter/blur хадгална, Esc буцна) |
| 2–3 | Modal (480–560px) эсвэл popover |
| 4–8 | Side sheet / drawer (560–640px) |
| >8 эсвэл олон хэсэгтэй | Тусдаа хуудас, URL-тай (`/orders/new`), max-width 720 |

- **Unsaved-changes guard**: өөрчлөлттэй байхад навигаци, modal хаах, tab хаах үед асуу (`beforeunload` + router guard). Асуулт: «Хадгалаагүй өөрчлөлт байна» · [Үлдэх] [Хаях] — «Болих» гэсэн хоёрдмол үг хэрэглэхгүй.
- Validation мөч, error summary, input төрлүүд — 06-components.md → Форм.

## Feedback ба хугацаа

| Хугацаа | Шаардлага |
|---|---|
| ≤100ms | Шууд хариу мэт — ямар ч indicator хэрэггүй (hover, toggle, inline edit) |
| ≤1s | Урсгал тасрахгүй — товч loading төлөвт, курсор/spinner |
| ≤10s | Анхаарал хадгална — skeleton эсвэл progress; юу болж байгааг текстээр |
| >10s | **Progress bar + хувь/ETA**, цуцлах боломж, background-д шилжүүлж дуусахад мэдэгдэл |

- **Skeleton-ийг 300ms хойшлуул** (хурдан хариу дээр анивчихгүй); skeleton гарсан бол ≥500ms байлга (флаш хоёр талдаа).
- **Optimistic UI** зөвхөн: (1) амжилтын магадлал өндөр, (2) буцаах хялбар, (3) хариу ≤1s. Төлбөр, устгах, илгээх — optimistic биш. Алдвал төлөвийг буцааж, **алдааг тухайн элементийн дэргэд** харуул.
- Stale data, offline, rate-limit — 14-app-resilience.md.

## Destructive үйлдэл ба баталгаажуулалт

| Үйлдлийн эрсдэл | Хэв маяг |
|---|---|
| Буцаах боломжтой (нэг бичлэг устгах, архивлах) | Шууд гүйцэтгээд **Undo toast 5с** (soft delete, 5с дараа л бодит) |
| Буцаахгүй, дунд зэрэг (төслөө устгах, гишүүн хасах) | Modal confirm |
| Bulk (олон мөр) | Confirm + **Undo 10с** хоёулаа |
| Сүйрлийн (бүх өгөгдөл, аккаунт, tenant, production DB) | Type-to-confirm |

- **Эхний сонголт soft delete + Undo** — confirm modal-ыг хэрэглэгч уншихгүй дардаг; Undo нь ойлгож амждаг.
- Modal confirm: гарчиг нь асуулт + объектын нэр («“Q3 тайлан” төслийг устгах уу?»), ерөнхий «Итгэлтэй байна уу?» биш; body-д үр дагавар 1 өгүүлбэр.
- Confirm товч нь үйлдлийн үг («Устгах»), «OK»/«Тийм» биш; destructive бол улаан, баруун талд; «Болих» зүүнд; default фокус «Болих» дээр.
- Type-to-confirm: объектын нэрийг яг бичүүлнэ, бичтэл товч disabled; copy-paste-ийг зориуд хаахгүй (хүртээмж).
- **Давхар баталгаажуулалт хэзээ ч байхгүй** (modal → дахин modal) — хоёр дахь нь эхнийхийн утгыг устгана.
- Олон зүйл устгахад тоог бич: «14 файл устгах».
- Буцаах боломжгүйг «Энэ үйлдлийг буцаах боломжгүй» гэж тодорхой хэл, зөвхөн ингэж хэлэх ёстой үед.

## Эрхгүй (permission-denied) төлөв

| Дүрэм | Хэзээ |
|---|---|
| **Нуух** | Үүрэгт огт хамааралгүй модуль/цэс (кассчинд «Billing settings»); нуугаад хуудсыг хоосон орхихгүй |
| **Disable + tooltip** | Тухайн контекстэд байх ёстой, гэхдээ одоо хориотой үйлдэл («Зөвхөн админ баталгаажуулна») |
| **Харуулж тайлбарлах** | Хуудас руу шууд URL-аар орсон: 403 хуудас — хэн зөвшөөрөл өгөхийг + хүсэлт илгээх товч (14-app-resilience.md) |

Permission-denied нь компонентын 5 төлөвийн нэг (06-components.md) — хоосон хуудас биш.

## Хоосон төлөвийн хоёр төрөл

| | First-run (анхны) | Filtered (шүүлтүүрийн үр дүн 0) |
|---|---|---|
| Copy | Юу болох, яагаад хэрэгтэйг 1–2 өгүүлбэр | «“xyz”-д таарах бичлэг алга» |
| CTA | Primary «Шинээр үүсгэх» + «Импортлох»/«Заавар» | «Шүүлтүүрийг цэвэрлэх» (secondary) |
| Зураг | Illustration болно, ≤200px | Зураггүй, icon хангалттай |
| Өндөр | Бүтэн content талбай | Table header үлдэж, доор нь 160–240px |

- Filtered хоосон төлөвт table-ийн header, шүүлтүүр chip-үүд **алга болохгүй** — үгүй бол цэвэрлэх аргагүй.

## Keyboard ба command palette

- Global: `?` shortcut жагсаалт · `/` хайлтад focus · `Esc` modal/drawer/popover хаах · **`Cmd/Ctrl+K` command palette**. Platform-ыг таниад зөвхөн тохирох тэмдэгтийг харуул (`⌘K` vs `Ctrl K`).
- Shortcut-ыг tooltip-д давхар харуул: «Хадгалах ⌘S». Single-key shortcut (`g` `i` мэт) текст талбарт идэвхгүй; browser-ийн default-тай зөрчилдөхгүй (`Cmd+L`, `Cmd+T`, `Cmd+W` бүү авч хэрэглэ).
- `?` help sheet: shortcut-уудыг бүлэглэн (Navigation / Edit / View) хүснэгтээр.
- **Command palette**: навигаци + үйлдэл + хайлт; fuzzy match (дараалал хадгалсан үсэг тааруулах), typo-д тэсвэртэй; хайлтын талбар дээр, үр дүн ≤10 харагдана, scroll-тай. Хоосон үед **сүүлд ашигласан 5–8 үйлдэл** эхэнд, дараа нь бүлэг тус бүрийн түгээмэл. Мөр бүр: icon + нэр + (баруун талд) shortcut hint muted; бүлгийн гарчиг 12px muted. Өргөн 560–640px, дэлгэцийн дээрээс 15–20% зайд; `Esc` хаана, фокус trigger руу буцна. Focus дүрэм — 07-accessibility.md.

## Notifications · Multi-tenant · Audit

- **Notifications center**: top bar icon + тоо badge → popover 360–400px, сүүлийн 10–20, «Бүгдийг үзэх» → хуудас. Мөр бүр: avatar · текст (`line-clamp: 2`) · relative цаг · уншаагүй цэг; «Бүгдийг уншсан болгох». Toast түр, center нь архив.
- **Multi-tenant**: switcher sidebar-ын дээд хэсэгт, ≥8 tenant бол хайлттай; **tenant нэр бүх хуудсанд ил** (нэг tenant-тай ч нэр үлдэнэ). Орчин (staging) ба impersonation төлөв — хаагдахгүй өнгөт баннер (32–40px) дээд талд.
- **Audit / History**: detail-ийн «Түүх» tab эсвэл баруун sidebar-т timeline: avatar · «**Бат** төлөвийг *Шинэ → Баталгаажсан* болгосон» · цаг (`title`-д абсолют). Бичлэг = хэн · юу (хуучин → шинэ) · хэзээ; урт текстэд «Ялгааг харах» diff; шинээс хуучин руу, 20-оор.

## Хуудас бүрт байх ёстой

1. `h1` хуудасны нэр + primary action баруун дээд, нэг мөрөнд.
2. Гүн ≥3 бол breadcrumb; sidebar-т active цэс bar + weight-тэй.
3. 5 төлөв: loading (300ms-ийн дараах skeleton) / empty (first-run ба filtered тусдаа) / error (retry-тэй) / success / permission-denied.
4. Шүүлтүүр, сорт, хуудас, tab, хайлт — бүгд URL-д; density/sidebar localStorage-д; аккаунтын тохиргоо серверт.
5. Destructive үйлдэл: Undo 5с (bulk 10с) эсвэл confirm (үйлдлийн нэртэй товч); буцаашгүй бол type-to-confirm.
6. Хадгалаагүй өөрчлөлтийн guard.
7. Эрхгүй үйлдэл: нуух/disable+tooltip/тайлбарлах дүрмийн аль нэг нь ухамсартай сонгогдсон.
8. Tenant нэр ба орчны баннер ил.
9. `Esc` хаана, `/` хайлт, `Cmd+K` palette, focus-visible ажиллана.
10. ≤1024px-д sidebar drawer, table эхний багана sticky, action-ууд overflow-д.
11. Route солигдоход title + focus + announce; offline/session-expiry/stale баннерууд (14-app-resilience.md).

## Эх сурвалж

- Nielsen Norman Group — "Response Times: The 3 Important Limits"; "Breadcrumbs: 11 Design Guidelines"; "Confirmation Dialogs Can Prevent User Errors — If Not Overused"; "Infinite Scrolling: When to Use It, When to Avoid It"; "Search-Log Analysis"; "Drop-Down Menus: Use Sparingly" (nngroup.com/articles)
- web.dev — "RAIL model" (web.dev/articles/rail)
- WCAG 2.2 — 1.4.1 Use of Color, 2.4.8 Location, 2.5.8 Target Size (Minimum), 3.3.4 Error Prevention, 3.3.8 Accessible Authentication (w3.org/TR/WCAG22)
- Material Design 3 — "Navigation drawer", "Navigation rail", "Data tables" (m3.material.io/components)
- Apple HIG — "Sidebars", "Undo and redo", "Loading", "Searching" (developer.apple.com/design/human-interface-guidelines)
- IBM Carbon Design System — "Data table", "Pagination", "Notifications" patterns (carbondesignsystem.com)
- Shopify Polaris — Pagination, IndexTable (bulk actions) (polaris.shopify.com/components)
- GOV.UK Design System — Pagination, Search; Patterns: Confirm before deleting
- Refactoring UI — Adam Wathan & Steve Schoger (hierarchy, empty states, table design бүлгүүд)
