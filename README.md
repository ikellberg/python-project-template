# Python Project Template

Шаблон для Python-проектов.

## Структура

```
project/
├── .agents-config/           # Источник правды для правил и скиллов
│   ├── RULES.md              # Базовые правила для LLM
│   └── skills/               # Скиллы для LLM
├── .claude/                   # Claude Code — симлинки на .agents-config
│   ├── CLAUDE.md -> ../.agents-config/RULES.md
│   └── skills -> ../.agents-config/skills
├── .codex/                   # Codex — симлинки на .agents-config
│   ├── AGENTS.md -> ../.agents-config/RULES.md
│   └── skills -> ../.agents-config/skills
├── .cursor/                  # Cursor — симлинки + правила
│   ├── skills -> ../.agents-config/skills
│   └── rules/dev_rules.mdc   # Правила (дублирует RULES для Cursor)
├── docs/
│   ├── prd.md                # Product Requirements Document (заполняется скиллом /prd)
│   ├── stories/              # User Stories (US-XXX.md)
│   │   └── done/             # Завершённые
│   ├── issues/               # Issues (ISS-XXX.md)
│   │   └── done/             # Закрытые
│   ├── plans/                # Планы реализации
│   ├── reviews/              # Ревью планов и кода
│   └── meetings/             # Сводки встреч
├── src/project_name/         # Код проекта
├── tests/
├── data/                     # Данные для разработки (не в git)
├── project_state.md          # Архитектура, спринт, backlog, done, грабли
└── pyproject.toml
```

### Симлинки

Claude, Codex и Cursor берут правила и скиллы из `.agents-config/` через симлинки. Меняешь `.agents-config/` — обновляется везде.

### Что куда

| Что | Куда |
|-----|------|
| Правила, скиллы | `.agents-config/` |
| PRD | `docs/prd.md` |
| User stories | `docs/stories/US-XXX.md` |
| Issues (баги, tech debt) | `docs/issues/ISS-XXX.md` |
| Планы реализации | `docs/plans/` |
| Ревью | `docs/reviews/` |
| Сводки встреч | `docs/meetings/` |
| Состояние проекта | `project_state.md` |
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
- [ ] Не заполняй `docs/prd.md`, `project_state.md`, `description` в pyproject.toml  
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
