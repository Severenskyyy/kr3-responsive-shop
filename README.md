# SVR — адаптивная и доступная версия

## Контрольная работа №3

Тема: «Адаптивная вёрстка и доступность многостраничного HTML/CSS-сайта».

Основа — HTML/CSS-проект из КР №1 (без Bootstrap). Сайт адаптирован под экраны от 360 px до широких мониторов
и проверен на доступность.

**Студент:** Кирбитов Артём Евгеньевич, группа ЭФБО-04-25

## Ссылка на опубликованный проект

- GitHub Pages: https://severenskyyy.github.io/kr3-responsive-shop/
- Репозиторий: https://github.com/Severenskyyy/kr3-responsive-shop

## Структура проекта

```
kr3-responsive-shop/
├── index.html, catalog.html, product.html, order.html, contacts.html, success.html
├── css/
│   ├── style.css         базовые стили — сразу для телефона (Mobile First)
│   └── responsive.css    медиазапросы min-width: 480 / 640 / 768 / 1024 / 1440 px
├── fonts/                шрифты Unbounded и Onest
├── images/               фото в 2–3 размерах и форматах AVIF, WebP, JPEG
├── lighthouse/
│   ├── accessibility-report.html     отчёт Lighthouse для index.html
│   ├── accessibility-catalog.html    отчёт для catalog.html
│   ├── accessibility-order.html      отчёт для order.html
│   └── accessibility-report.png      скриншот трёх результатов
├── docs/screens/         скриншоты главной на проверенных ширинах
├── README.md
└── .gitignore
```

JavaScript не используется: меню и модальное окно работают на встроенных возможностях HTML (Popover API и Invoker Commands).

## Адаптивные решения

- добавлен `<meta name="viewport" content="width=device-width, initial-scale=1.0">` на все страницы;
- добавлен файл `css/responsive.css`, подключён после `style.css` на всех страницах;
- применён подход **Mobile First**: в `style.css` раскладка для телефона, медиазапросы `min-width` её расширяют;
- **навигация:** на телефоне и планшете — кнопка «Меню», которая открывает боковую панель (Popover API), с 1024 px — обычное меню в шапке;
- **hero-блок:** на телефоне текст и фото друг под другом, плашка сборки заходит на фото; с 768 px — две колонки;
- **категории:** 2 плитки в ряд на телефоне, 4 — с 768 px;
- **карточки:** 1 колонка → 2 (с 480 px) → 3 в каталоге и 4 на главной (с 1024 px);
- **каталог и фильтр:** на телефоне категории — горизонтальная лента «чипсов», цена/бренд/наличие свёрнуты в `details`;
  с 1024 px фильтр — боковая колонка, которая «прилипает» при прокрутке;
- **страница товара:** галерея и описание друг под другом, меню разделов прокручивается по горизонтали внутри себя;
- **форма:** на телефоне одна колонка, поля высотой 48 px, кнопки во всю ширину; с 640 px — две колонки;
- **таблицы:** характеристики остаются таблицей; сравнение сборок на телефоне превращается в **карточки** (подписи из `data-label`),
  с 640 px снова таблица внутри `.table-wrapper` с горизонтальной прокруткой;
- **изображения:** `img, video { max-width: 100%; height: auto; }`, `srcset` + `sizes` для всех фото,
  `<picture>` с форматами AVIF и WebP, разные кадры hero для телефона и десктопа (art direction);
- **типографика:** размеры заголовков через `clamp()` — плавно меняются между телефоном и десктопом;
- горизонтальной прокрутки всей страницы нет ни на одной ширине.

## Проверенные ширины

| Ширина | Устройство | Результат |
| --- | --- | --- |
| 360 px | телефон | всё в одну колонку, нет горизонтального скролла |
| 430 px | крупный телефон | нет горизонтального скролла |
| 768 px | планшет | hero в две колонки, категории в ряд, телефон в шапке |
| 1024 px | небольшой десктоп | полное меню, боковой фильтр, 3 карточки в ряд |
| 1440 px | широкий десктоп | контейнер 1320 px, 4 карточки на главной |

Проверялись все страницы: шапка, навигация, hero, сетка карточек, каталог, фильтр, товар, таблицы, формы, контакты, подвал, модальное окно.
Скриншоты главной — в `docs/screens/`.

## Доступность

- добавлена ссылка «Перейти к основному содержимому» (skip-link) — первая по Tab;
- на каждой странице один `h1`, заголовки идут по порядку;
- видимый фокус `:focus-visible` у всех интерактивных элементов, на тёмном фоне — светлая рамка;
- у всех полей формы есть `label`, подсказки и ошибки связаны с полем через `aria-describedby`;
- у всех содержательных фото осмысленный `alt`, у декоративных — пустой `alt=""`, иконки скрыты `aria-hidden="true"`;
- контраст текста не ниже 4.5:1, рамок полей — не ниже 3:1;
- текущая страница в меню и хлебных крошках отмечена `aria-current="page"`;
- модальное окно переделано на `<dialog>`: фокус переходит внутрь, удерживается в окне, Esc закрывает, фокус возвращается на кнопку;
- у ссылок «Подробнее» есть скрытое уточнение «про SVR Gamer» и т. п., у цены — «Цена:», у старой цены — «Цена без скидки»;
- прокручиваемая таблица доступна с клавиатуры (`tabindex="0"`, `role="region"` с названием);
- учитываются системные настройки: `prefers-reduced-motion` (без анимаций) и режим высокой контрастности (`forced-colors`);
- ARIA используется только там, где нет подходящего HTML.

## Lighthouse

Проверены `index.html`, `catalog.html`, `order.html`, категория Accessibility: **100 / 100 / 100**.
Отчёты и скриншот сохранены в папке `lighthouse/`.

Дополнительно для главной: Performance 93, Accessibility 100, Best Practices 100, SEO 100 (мобильный режим),
на десктопе Performance 99. Проверка запускалась локально; повторить на GitHub Pages можно в Chrome:
DevTools → Lighthouse → Accessibility → Analyze page load.

Lighthouse не заменяет ручную проверку: дополнительно сайт пройден с клавиатуры (Tab, Shift+Tab, Enter, Esc, стрелки в галерее)
и проверен на всех ширинах из таблицы выше.

## Самостоятельные доработки

1. **Мобильное меню на Popover API** — без JavaScript: боковая панель, затемнение фона, закрытие по Esc и клику вне меню.
2. **Таблица сравнения превращается в карточки** на телефоне, а не только прокручивается.
3. **Несколько вариантов responsive images:** AVIF и WebP через `<picture>` и разные кадры hero для телефона и десктопа.
4. **Доступное модальное окно** на `<dialog>` и Invoker Commands (`command="show-modal"`), skip-link, `aria-describedby` у полей.

## Проверка

- HTML и CSS проверены валидатором W3C (Nu Html Checker) — ошибок нет.
- Все внутренние ссылки и якоря ведут на существующие страницы и элементы.

## Источники изображений

Все фото взяты из открытых источников, названия товаров соответствуют моделям на снимках.
У собственных сборок SVR название придумано, а в характеристиках указаны комплектующие, которые видны на фото
(ASUS ROG, Thermaltake, G.Skill Trident Z RGB, MSI; ASUS PRIME Z390-A, Cooler Master, Corsair; AMD Ryzen Threadripper, XFX Radeon RX 6700 XT, Lian Li).

| Файлы | Что на фото | Автор / источник | Лицензия | Ссылка |
| --- | --- | --- | --- | --- |
| `p-svr-gamer, hero, g-gamer-red` | ПК с красной подсветкой | Unsplash | Unsplash License | https://images.unsplash.com/photo-1587202372583-49330a15584d |
| `g-gamer-blue, c-pc` | ПК с синей подсветкой | Unsplash | Unsplash License | https://images.unsplash.com/photo-1587202372775-e229f172b9d7 |
| `g-gamer-inside, g-gamer-detail` | Внутри ПК: ASUS ROG, Thermaltake, G.Skill | Unsplash | Unsplash License | https://images.unsplash.com/photo-1587202372634-32705e3bf49c |
| `p-svr-start` | ASUS PRIME Z390-A, Cooler Master, Corsair | Unsplash | Unsplash License | https://images.unsplash.com/photo-1660855552442-1bae49431379 |
| `p-svr-studio` | Threadripper, XFX RX 6700 XT, Lian Li | Unsplash | Unsplash License | https://images.unsplash.com/photo-1624705002806-5d72df19c3ad |
| `p-rog-strix-g15` | ASUS ROG Strix G15 G513QY | D-Kuru, Wikimedia Commons | CC BY-SA 4.0 | https://commons.wikimedia.org/w/index.php?curid=112212246 |
| `c-laptops` | ASUS ROG Strix G15 G513QY, крышка | D-Kuru, Wikimedia Commons | CC BY-SA 4.0 | https://commons.wikimedia.org/w/index.php?curid=112212272 |
| `p-macbook-air-m2` | Apple MacBook Air M2 | KKPCW (Kyu3), Wikimedia Commons | CC BY-SA 4.0 | https://commons.wikimedia.org/w/index.php?curid=121262188 |
| `p-dell-xps-13` | Dell XPS 13 | Unsplash | Unsplash License | https://images.unsplash.com/photo-1593642702821-c8da6771f0c6 |
| `p-hyperx-cloud` | HyperX Cloud | GEEK KAZU, Flickr | CC BY 2.0 | https://www.flickr.com/photos/152342724@N04/40218660592 |
| `p-sony-wh-1000xm4, c-audio` | Sony WH-1000XM4 | Unsplash | Unsplash License | https://images.unsplash.com/photo-1618366712010-f4ae9c647dcb |
| `p-airpods-pro` | Apple AirPods Pro | Unsplash | Unsplash License | https://images.unsplash.com/photo-1572569511254-d8f925fe2cbb |
| `p-logitech-g502, c-gear` | Logitech G502 | Unsplash | Unsplash License | https://images.unsplash.com/photo-1563297007-0686b7003af7 |
| `p-keychron-k2` | Keychron K2 | Unsplash | Unsplash License | https://images.unsplash.com/photo-1618384887929-16ec33fab9ef |
| `p-dualshock-4` | Sony DualShock 4 | Unsplash | Unsplash License | https://images.unsplash.com/photo-1592840496694-26d035b52b48 |

Шрифты Unbounded и Onest — SIL Open Font License, подключены локально из пакетов Fontsource.
