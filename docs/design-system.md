# Design System

Здесь описано, как мы работаем с дизайном на уровне компонентов и
токенов. Этот документ — источник правды для **визуальной части**
любого клиентского сайта.

---

## Design Tokens (CSS Variables)

Все цвета, размеры и отступы хранятся в `app/globals.css` как
CSS-переменные. Tailwind конфигурируется так, чтобы использовать эти
переменные — это даёт единый источник истины.

### Минимальный набор токенов в `:root`

```css
@layer base {
  :root {
    /* Colors — semantic naming */
    --color-bg: 255 255 255;            /* основной фон */
    --color-bg-muted: 248 248 246;      /* фон секций второго плана */
    --color-fg: 23 23 23;               /* основной текст */
    --color-fg-muted: 115 115 115;      /* вторичный текст */
    --color-primary: 22 101 52;         /* брендовый акцент */
    --color-primary-fg: 255 255 255;    /* текст на акценте */
    --color-border: 229 229 229;        /* границы */

    /* Typography */
    --font-display: "Fraunces", serif;  /* заголовки */
    --font-body: "Inter", sans-serif;   /* основной текст */
    --font-mono: "JetBrains Mono", monospace; /* техн./метки */

    /* Radii */
    --radius-sm: 0.375rem;
    --radius-md: 0.75rem;
    --radius-lg: 1.25rem;

    /* Spacing scale (через Tailwind, не дублируем) */
  }

  .dark {
    --color-bg: 10 10 10;
    --color-fg: 245 245 245;
    /* и т.д. */
  }
}
```

### Под каждого клиента — меняем токены, не код

Когда стартует новый проект, ты в первую очередь подбираешь палитру
и шрифты под бренд клиента и обновляешь `:root` переменные. **Код
компонентов не трогаешь** — они автоматически пересобираются под
новый бренд.

---

## Typography Approach

**Правило двух шрифтов:** один для заголовков (display), один для
основного текста (body). Третий — моноширинный — опционально для
технических меток, цен, дат.

**Источник:** Google Fonts через `next/font` (это даёт оптимизацию и
zero-CLS).

**Подбор шрифтов под нишу:**

| Ниша | Display | Body |
|---|---|---|
| Бутик-отель / премиум | Fraunces / Cormorant Garamond | Inter / Manrope |
| Современный B2B | Inter / Geist | Inter |
| Ресторан, кафе | Playfair Display / DM Serif | Lora / Source Sans |
| Глэмпинг, природа | Cormorant / Tinos | Nunito / Karla |
| Tech-стартап | Geist / Space Grotesk | Geist |

---

## Iconify (обязательно)

**Никогда не рисуем SVG-иконки сами и не используем эмодзи как
иконки.** Только Iconify через web component.

### Подключение (один раз в `app/layout.tsx`)

```tsx
import Script from 'next/script'

export default function RootLayout({ children }) {
  return (
    <html>
      <head>
        <Script
          src="https://code.iconify.design/iconify-icon/2.1.0/iconify-icon.min.js"
          strategy="afterInteractive"
        />
      </head>
      <body>{children}</body>
    </html>
  )
}
```

### Использование

```tsx
// UI-иконка
<iconify-icon icon="solar:home-2-linear" width="24"></iconify-icon>

// Логотип бренда
<iconify-icon icon="simple-icons:booking" width="32"></iconify-icon>
```

### Какие иконки выбирать

- **UI-иконки:** `solar:*-linear` (Solar Linear set) — единый стиль,
  тонкие линии, премиум-ощущение
- **Брендовые логотипы:** `simple-icons:*` — все известные бренды
- **Альтернативы для разнообразия:** `lucide:*`, `tabler:*` (если Solar
  не подходит стилистически — например, для тех-проектов)

Каталог: https://icon-sets.iconify.design/

---

## Reference Analysis Checklist

Когда у тебя есть референс (скриншот / ссылка / макет из Stitch),
**не копируй его напрямую**. Прогоняй через чек-лист:

### 1. Цветовая палитра
- Извлеки 4-6 основных цветов с HEX-кодами
- Определи: основной фон, вторичный фон, основной текст, вторичный
  текст, акцентный цвет, цвет границ

### 2. Типографика
- Шрифты заголовков и текста (определи по визуальным признакам:
  serif/sans-serif, плотность, контраст)
- Если не очевидно — подбери ближайший Google Font
- Размеры: какой размер у H1, H2, body, caption

### 3. Spacing & Layout
- Какой контейнер по ширине (узкий 768px / средний 1024px / широкий 1280px+)
- Отступы между секциями (большие = премиум, маленькие = плотный
  контент)
- Сетка — это flex или grid? Сколько колонок?

### 4. Углы и тени
- Border-radius: острые / средние / большие
- Тени: есть / нет / тонкие / выраженные

### 5. Анимация и motion
- Какие элементы движутся при скролле (parallax / fade-in /
  slide-up)
- Hover-эффекты (увеличение / смена цвета / появление подложки)

### 6. Что **НЕ** копируем
- Конкретные изображения и иллюстрации (только настроение)
- Конкретные тексты
- Логотип

---

## Section-Building Pattern

Каждая секция — это **независимый React-компонент в
`/components/sections/`**.

### Стандартная структура файла секции

```tsx
// components/sections/hero.tsx
import { Container } from '@/components/ui/container'
import { Button } from '@/components/ui/button'

interface HeroProps {
  title: string
  subtitle?: string
  ctaText: string
  ctaHref: string
  imageUrl: string
}

export function Hero({ title, subtitle, ctaText, ctaHref, imageUrl }: HeroProps) {
  return (
    <section className="relative min-h-[80vh] flex items-center">
      <Container>
        {/* содержимое */}
      </Container>
    </section>
  )
}
```

**Принципы:**
- Все данные приходят пропсами — секция переиспользуема между
  клиентами
- Не зашиваем тексты в JSX — берём из `useTranslations()` или из
  пропсов
- Не зашиваем цвета в Tailwind напрямую — используем семантические
  классы из токенов (`bg-background`, `text-foreground`)

---

## Animation Approach

Используем **Framer Motion** для анимаций. По умолчанию — сдержанные
эффекты:

- Появление при скролле: `fade-in + slide-up` (8px) на 0.5-0.7s
- Hover на карточки: `scale 1.02` + изменение `border-color`
- Не используем: вращающиеся элементы, чрезмерный parallax, скачущий
  текст. Это снижает воспринимаемое качество сайта.

---

## Reference Sources (Curated)

Когда нужны идеи для секций под нишу:

- **Bentogrids** — bento-структуры (бенто-сетка карточек разного
  размера, тренд от Apple)
- **Mobbin** — категоризированная библиотека секций. Бесплатная
  регистрация. Лучший источник.
- **Supahero** — только Hero-секции, удобно для старта
- **h1gallery** — фокус на типографике заголовков
- **CTA.Gallery** — варианты призывов к действию
- **Land-book.com** — целые лендинги, отсортированные по нише

---

## Quick Brand Adaptation Workflow

Когда у нового клиента есть бренд (логотип, цвета):

1. Загрузить логотип в `/public/images/logo.svg`
2. Извлечь основные 2-3 цвета из логотипа → обновить
   `--color-primary` и связанные в `globals.css`
3. Подобрать пару шрифтов под характер бренда (см. таблицу выше)
4. Запустить dev-сервер и убедиться, что все компоненты
   автоматически перекрасились
5. Если нет — значит где-то жёстко зашит цвет, исправить
