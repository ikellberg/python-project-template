# Project Template

Шаблон для dev-проектов. Stack-agnostic — стек добавляется при инициализации.

## Структура

```
project/
├── .claude/                  # Claude Code
│   └── skills/               # dev-скиллы (симлинки через sync-dev-skills.sh)
├── .codex/                   # Codex
│   └── skills/               # dev-скиллы (симлинки через sync-dev-skills.sh)
├── docs/
│   ├── prd.md                # Product Requirements Document
│   ├── epics/                # Эпики проекта (01-slug.md, ...)
│   ├── notes/                # Заметки, design-документы, исследования
│   ├── stories/              # User Stories (US-XXX.md)
│   │   └── done/
│   ├── issues/               # Issues (ISS-XXX.md)
│   │   └── done/
│   ├── plans/                # Планы реализации
│   ├── reviews/              # Ревью планов и кода
│   └── meetings/             # Сводки встреч
├── src/                      # Код проекта
├── tests/                    # Тесты
├── data/                     # Данные для разработки (не в git)
├── PROJECT_STATE.md          # Архитектура, спринт, backlog, done, грабли
├── CLAUDE.md                 # → ~/.agents/DEV-RULES.md
└── AGENTS.md                 # → ~/.agents/DEV-RULES.md
```

### Симлинки

Claude и Codex берут правила и скиллы из `~/.agents/`:

| Файл | Назначение |
|---|---|
| `CLAUDE.md` → `~/.agents/DEV-RULES.md` | Инженерные правила для Claude |
| `AGENTS.md` → `~/.agents/DEV-RULES.md` | Инженерные правила для Codex |
| `.claude/skills/*` | Индивидуальные симлинки на `~/.agents/dev-skills/*` |
| `.codex/skills/*` | Индивидуальные симлинки на `~/.agents/dev-skills/*` |

Скиллы синхронизируются скриптом: `~/.agents/sync-dev-skills.sh`

### Что куда

| Что | Куда |
|-----|------|
| PRD | `docs/prd.md` |
| Эпики проекта | `docs/epics/` |
| Design-документы, исследования | `docs/notes/` |
| User Stories | `docs/stories/US-XXX.md` |
| Issues (баги, tech debt) | `docs/issues/ISS-XXX.md` |
| Планы реализации | `docs/plans/` |
| Ревью | `docs/reviews/` |
| Сводки встреч | `docs/meetings/` |
| Состояние проекта | `PROJECT_STATE.md` |

## После копирования шаблона

На этом этапе подготовь репозиторий к работе.
Контекста о продукте ещё нет — **ничего не проектируй и не заполняй**.

### Обязательные шаги

- [ ] Добавь конфиг стека (pyproject.toml / package.json / go.mod / ...)
- [ ] Добавь stack-specific записи в `.gitignore`
- [ ] Настрой структуру `src/` и `tests/` под стек
- [ ] Запусти `~/.agents/sync-dev-skills.sh` для синхронизации dev-скиллов
- [ ] Проверь, что симлинки на `~/.agents/` работают

### Не делать на этом этапе

- Не заполняй `docs/prd.md`, `PROJECT_STATE.md`
- Не пиши README продукта — удали этот файл или замени на пустой

Цель этапа — **чистая, нейтральная стартовая кодовая база**, готовая к дальнейшей работе.
