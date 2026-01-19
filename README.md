# Python Project Template

Шаблон для Python-проектов.

## Структура и что куда класть

```
project/
├── src/project_name/     # Код проекта
│   ├── agents/           # Агенты (если есть)
│   └── main.py           # Точка входа
├── tests/                # Тесты
├── data/                 # Данные для разработки (не попадает в git)
├── docs/                 # Документация (аудиты, архитектура, ADR)
├── .claude/              # Правила для Claude
│   ├── CLAUDE.md         # Как писать код, принципы
│   └── skills/           # Навыки для LLM
├── .cursor/rules/        # Правила для Cursor
├── project_state.md      # Описание проекта, план, прогресс
└── pyproject.toml        # Зависимости, настройки линтера
```

### Что куда:

| Что | Куда |
|-----|------|
| Описание проекта, план, статус | `project_state.md` |
| Правила для LLM (как писать код) | `.claude/CLAUDE.md` |
| Аудиты проекта | `docs/audit_YYYY-MM-DD.md` |
| Архитектурные решения | `docs/` |
| Тестовые данные (PDF, JSON) | `data/` |
| Код | `src/project_name/` |
| Тесты | `tests/` |

## После создания проекта

- [ ] Переименуй `src/project_name/` под свой проект
- [ ] Обнови `pyproject.toml` (name, description)
- [ ] Заполни `project_state.md` (описание, план)
- [ ] Заполни `.claude/CLAUDE.md` (правила)
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
