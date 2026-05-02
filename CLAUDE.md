# Project Conventions

Этот файл читается Claude Code в начале каждой сессии. Здесь — все
постоянные правила для работы в этом проекте.

---

## Stack

- **Framework:** Next.js 15 (App Router)
- **Styling:** Tailwind CSS + CSS variables в `globals.css` для design tokens
- **UI Components:** shadcn/ui (через `npx shadcn@latest add`)
- **Language:** TypeScript (strict mode)
- **Icons:** Iconify (`<iconify-icon>` web component) — Solar Linear для UI,
  Simple Icons для брендов
- **Forms:** React Hook Form + Zod для валидации, Resend для отправки на email
- **Internationalization:** next-intl (минимум RU + EN, для отелей часто +UZ)
- **Deploy:** Vercel
- **AI integrations:** webhook → n8n → Gemini API (для AI-консьержей и
  других ассистентов)

---

## Folder Structure

```
/app                  # Next.js App Router страницы
  /[locale]           # Локализованные страницы
/components
  /ui                 # shadcn-компоненты (не редактируем напрямую)
  /sections           # Секции страниц (Hero, Features, Pricing и т.д.)
  /widgets            # Виджеты (AI-консьерж, формы)
/content              # Markdown-контент сайта (для лёгкого редактирования)
/lib                  # Утилиты, хелперы
/public/images        # Статичные изображения
/messages             # Переводы (ru.json, en.json, uz.json)
/scripts              # Служебные скрипты (парсер Booking, генераторы)
/docs                 # Документация проекта (НЕ деплоится)
```

---

## Code Style

- **TypeScript:** не используй `any`, типизируй явно. Для пропсов
  компонентов — `interface ComponentNameProps`.
- **Компоненты:** функциональные, default export для страниц,
  named export для переиспользуемых компонентов.
- **Tailwind классы:** группируй по типу (layout → spacing → typography
  → colors → effects). Для длинных списков используй `cn()` из
  `lib/utils`.
- **Файлы:** kebab-case для имён (`hero-section.tsx`), PascalCase для
  имён компонентов внутри.
- **Импорты:** используй абсолютные пути от корня (`@/components/...`),
  не относительные (`../../...`).
- **Комментарии:** комментируй "почему", а не "что". Код должен
  объяснять "что" сам.

---

## Design Principles

- **Mobile-first.** Сначала верстаем под мобильный, потом усложняем.
- **CSS-переменные для design tokens.** Все цвета, основные размеры,
  отступы — через `:root` переменные в `globals.css`. Это даёт
  единообразие и лёгкую кастомизацию под клиента.
- **Минимализм.** Современный сдержанный стиль без перегруза эффектами.
  Анимации существуют, чтобы поддержать стиль, а не отвлекать.
- **Никакого Lorem Ipsum.** Все тексты должны быть релевантны проекту.
  Если контента ещё нет — генерируй осмысленные тексты на тему проекта.
- **Иконки только Iconify.** Не пытайся рисовать SVG-иконки сам —
  используй Solar Linear для UI и Simple Icons для брендов через
  web component:
  ```html
  <iconify-icon icon="solar:user-linear" width="24"></iconify-icon>
  ```
  Скрипт подключается один раз в `layout.tsx`.
- **Изображения:** для плейсхолдеров — Unsplash через `next/image`.
  Реальные фото клиента → `/public/images/` с осмысленными именами.

---

## Section-by-Section Workflow

Сайт собирается **по одной секции за раз**, не "сделай весь сайт
сразу". Подробности в `docs/section-by-section-workflow.md`.

Краткая суть:
1. Сначала Hero (она задаёт визуальный язык всему сайту)
2. Утвердили Hero → одна следующая секция
3. Каждая новая секция стилистически опирается на уже сделанные
4. После 2-3 секций структура и токены устаканиваются — дальше быстрее

---

## Languages

По умолчанию: русский + английский.
Для отелей в Узбекистане часто нужен узбекский.

Тексты выносятся в `/messages/{locale}.json`. В компонентах используется
`useTranslations()` из `next-intl`.

---

## Working with New Client Project

Когда стартует новый проект из этого шаблона:

1. **Спроси контекст:**
   - Ниша (отель / ресторан / другое)
   - Источники материала (Booking-ссылка / Instagram / сайт-референс /
     загруженные фото)
   - Языки сайта
   - Срок и бюджет (влияет на скоуп)

2. **Если ниша = отель** — обязательно прочитай:
   - `docs/hotel-domain-knowledge.md`
   - `docs/ai-concierge-system-prompt.md`

3. **Покажи план структуры страниц перед началом кодинга.** Не
   начинай сразу писать компоненты — сначала список страниц и
   секций на согласование.

4. **Стартуй с Hero-секции.** Используй методологию из
   `docs/section-by-section-workflow.md`.

---

## Specialised Agents

В `.claude/agents/` лежат специализированные саб-агенты. Активируй
их через `@agent-name` когда задача профильная:

- `@prompt-engineer` — универсальный инженер промптов для LLM
- `@hotel-concierge-engineer` — генерация системных промптов для
  AI-консьержей отелей

---

## What NOT to do

- **Не используй plain HTML/CSS/JS** для клиентских проектов. Это
  шаблон на Next.js — мы не теряем SEO, маршрутизацию, оптимизацию
  изображений ради "простоты".
- **Не вставляй стили инлайном** (`style={{...}}`) кроме случаев,
  когда значения динамические. Все статичные стили — через Tailwind.
- **Не плоди компоненты без необходимости.** Если секция используется
  один раз — можно не выделять в отдельный файл сразу.
- **Не редактируй файлы в `/components/ui/`** напрямую. Это shadcn —
  переустанавливай через CLI или оборачивай в свой компонент.
- **Не коммить `.env.local`, ключи API, webhook URLs.** Только
  `.env.example` с пустыми значениями.

---

## Reference Sites for Inspiration

Когда нужны референсы для дизайна секций:

- **Mobbin** (mobbin.com) — огромная библиотека секций по категориям
- **Bentogrids** (bentogrids.com) — bento-структуры (карточки разных
  размеров в сетке)
- **Supahero** (supahero.io) — Hero-секции
- **h1gallery** (h1gallery.com) — типографика и заголовки
- **CTA.Gallery** (cta.gallery) — призывы к действию

Подробнее с приёмами анализа референса → `docs/design-system.md`.
