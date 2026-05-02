# Web Base Template

Базовый шаблон для создания клиентских сайтов через Claude Code.

Стек: **Next.js 15 + Tailwind + shadcn/ui + TypeScript** на проде,
**n8n + Gemini API** для AI-консьержей.

---

## Как пользоваться

### Первый запуск (один раз)

1. Сделать этот репозиторий **template repository** на GitHub
   (Settings → Template repository → ✓)
2. Установить зависимости: Node.js 20+, pnpm, Vercel CLI
3. Настроить MCP-серверы для Claude Code: Stitch (через твой
   коннектор), Playwright (для парсинга Booking)

### Новый клиент

1. На GitHub: **Use this template** → создать новый репозиторий
   `client-{имя-отеля}`
2. Клонировать локально, открыть в VS Code с Claude Code
3. Сказать Claude Code:
   ```
   Стартую новый проект для клиента: {имя отеля}, {город}.
   Booking: {ссылка}.
   Языки: ru, en.
   Прочитай CLAUDE.md и docs/, действуй по протоколу.
   ```
4. Claude Code прочитает все правила и начнёт по протоколу из
   `docs/section-by-section-workflow.md`

---

## Что где лежит

```
.
├── CLAUDE.md                  ← главные правила (Claude читает первым)
├── README.md                  ← этот файл
│
├── .claude/
│   └── agents/
│       ├── prompt-engineer.md          ← универсальный промпт-инженер
│       └── hotel-concierge-engineer.md ← генератор промптов для AI-консьержей
│
└── docs/
    ├── design-system.md              ← дизайн-токены, Iconify, чек-лист
    ├── section-by-section-workflow.md ← методология "по секции за раз"
    ├── hotel-domain-knowledge.md     ← знания об отельной нише
    └── ai-concierge-system-prompt.md ← шаблон промпта для отельного AI
```

В будущем сюда добавятся:
- `app/`, `components/`, `lib/` — Next.js-проект (создаётся при
  первом запуске)
- `scripts/parse-booking.ts` — скрипт парсинга Booking-страниц
- `n8n-workflows/` — экспортированные JSON-воркфлоу для AI-консьержа
- `docs/restaurant-domain-knowledge.md`, `docs/clinic-domain-knowledge.md` —
  по мере появления новых ниш

---

## Активация саб-агентов

В Claude Code:

```
@prompt-engineer аудируй промпт в файле X
@hotel-concierge-engineer сгенерируй промпт для отеля Y
```

---

## Ключевые принципы (кратко)

1. **Сайт собирается по одной секции за раз**, начиная с Hero
2. **CSS-переменные** для всех цветов/шрифтов/отступов — единый
   источник стиля
3. **Iconify** для всех иконок (никогда не рисуем SVG сами)
4. **Никакого Lorem Ipsum** — релевантный контент с самого начала
5. **Mobile-first**, accessibility, SEO с первого коммита
6. **Под каждого клиента — отдельный репозиторий** из этого
   шаблона, не один проект на всех

Подробности — в `CLAUDE.md` и файлах `docs/`.

---

## Чек-лист добавления новой ниши

Когда захочешь делать сайты не только отелям (рестораны, клиники,
автосалоны):

1. Создать `docs/{niche}-domain-knowledge.md` по образцу
   `hotel-domain-knowledge.md` (специфика, термины, цены, типы клиентов)
2. Если для ниши нужен AI-ассистент — создать
   `docs/ai-{niche}-system-prompt.md` с шаблоном промпта
3. Если есть характерные секции — добавить в
   `components/sections/` версии под нишу
4. Создать саб-агент `.claude/agents/{niche}-concierge-engineer.md`
   если нужно
5. Дописать в `CLAUDE.md` блок про новую нишу с триггерами
   "если ниша = ресторан, прочитай..."

Базовый шаблон не дублируется — наследуется. Это и есть твоя
"производственная линия".
