# Python Project Template

Шаблон для Python-проектов.

## Структура

```
project/
├── .agents/                  # Источник правды для правил и скиллов
│   ├── RULES.md              # Базовые правила для LLM
│   └── skills/               # Скиллы для LLM
├── .claude/                  # Claude Code — симлинки на .agents
│   ├── CLAUDE.md -> ../.agents/RULES.md
│   └── skills -> ../.agents/skills
├── .codex/                   # Codex — симлинки на .agents
│   ├── AGENTS.md -> ../.agents/RULES.md
│   └── skills -> ../.agents/skills
├── docs/
│   ├── prd.md                # Product Requirements Document (скилл /prd)
│   ├── phases/               # Фазы проекта (01-slug.md, ...)
│   ├── notes/                # Заметки, design-документы, исследования
│   ├── stories/              # User Stories (US-XXX.md)
│   │   └── done/
│   ├── issues/               # Issues (ISS-XXX.md)
│   │   └── done/
│   ├── plans/                # Планы реализации
│   ├── reviews/              # Ревью планов и кода
│   └── meetings/             # Сводки встреч
├── src/project_name/         # Код проекта
├── tests/
├── data/                     # Данные для разработки (не в git)
├── PROJECT_STATE.md          # Архитектура, спринт, backlog, done, грабли
└── pyproject.toml
```

### Симлинки

Claude и Codex берут правила и скиллы из `.agents/` через симлинки. Меняешь `.agents/` — обновляется везде.

### Что куда

| Что | Куда |
|-----|------|
| Правила, скиллы | `.agents/` |
| PRD | `docs/prd.md` |
| Фазы проекта | `docs/phases/` |
| Design-документы, исследования | `docs/notes/` |
| User Stories | `docs/stories/US-XXX.md` |
| Issues (баги, tech debt) | `docs/issues/ISS-XXX.md` |
| Планы реализации | `docs/plans/` |
| Ревью | `docs/reviews/` |
| Сводки встреч | `docs/meetings/` |
| Состояние проекта | `PROJECT_STATE.md` |
| Код | `src/project_name/` |
| Тесты | `tests/` |

## После создания проекта (инициализация шаблона)

На этом этапе подготовь репозиторий к работе.
Контекста о продукте ещё нет — **ничего не проектируй и не заполняй**.

### Обязательные шаги

- [ ] Переименуй каталог `src/project_name/` в имя проекта
- [ ] Обнови `pyproject.toml`:
  - `name`
  - пути пакетов (`packages`)
  - entry points (`project.scripts`, если есть)
  - `known-first-party` (если используется)
- [ ] Проверь, что проект корректно собирается / импортируется после переименования

### README и документация

- [ ] Удали этот `README.md`
  Он относится только к шаблону и **не нужен в инициализированном проекте**
- [ ] Не заполняй `docs/prd.md`, `PROJECT_STATE.md`, `description` в pyproject.toml
  Всё это заполнится позже, когда будет контекст

Цель этапа — **чистая, нейтральная стартовая кодовая база**, готовая к дальнейшей работе.

## Команды

```bash
# Установка зависимостей
pip install -e ".[dev]"

# Линтер
ruff check src/

# Тесты
pytest
```
