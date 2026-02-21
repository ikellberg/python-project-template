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

## После создания проекта

- [ ] Переименуй `src/project_name/` под свой проект
- [ ] Обнови `pyproject.toml` (name, description)
- [ ] Заполни `docs/prd.md` (или вызови `/prd`)
- [ ] Заполни `project_state.md` (архитектура, backlog)
- [ ] Удали этот README или замени на описание своего проекта

## Команды

```bash
# Установка зависимостей
pip install -e ".[dev]"

# Линтер
ruff check src/

# Тесты
pytest
```
