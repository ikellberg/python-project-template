---
name: vps-docker-autodeploy
description: >
  Используй, когда пользователь просит настроить деплой, починить деплой,
  "деплой упал", "не деплоится", "настрой CI/CD", "добавь автодеплой",
  "workflow падает", или когда нужно что-то сделать с GitHub Actions, VPS,
  Docker Compose в контексте deploy pipeline.
  Триггеры -- настрой деплой, почини деплой, деплой упал, CI/CD, автодеплой,
  workflow падает, задеплой, /deploy.
---

# Автодеплой на VPS через Docker

Настраивай и отлаживай GitHub Actions -> VPS -> Docker Compose в одном из двух режимов:

- `setup`, если автодеплой ещё не настроен;
- `debug`, если workflow уже есть, но падает или деплой ведёт себя нестабильно.

## Когда использовать

- Нужно внедрить автодеплой на `main` для сервиса на VPS через Docker Compose.
- Нужно добавить безопасный `workflow_dispatch` с `dry_run`.
- Нужно подготовить deploy-пользователя, SSH-ключи и `DEPLOY_*` secrets.
- Нужно расследовать падение deploy workflow, health-check или rollback-сценарий.

## Когда не использовать

- Проект деплоится не через GitHub Actions -> VPS -> Docker Compose.
- Нужен только локальный Docker setup без CI/CD.
- Нужен общий инфраструктурный дизайн, а не настройка или отладка конкретного deploy workflow.

## Процесс

### 1. Собери контекст

Прочитай:

- `docker-compose.yml` или локальный аналог;
- `.github/workflows/*.yml`;
- `README.md` или локальные документы по deploy, rollback и recovery;
- `project_state.md`, issue, план, review или локальные проектные документы, если проект их использует.

Определи:

- режим `setup` или `debug`;
- директорию приложения на VPS;
- имя сервиса для health-check и логов;
- где лежит `.env`;
- какой механизм deploy уже есть или должен появиться;
- что должен сделать агент и что должен сделать пользователь.

Сначала сформируй краткий план и риски.

Остановись и дождись подтверждения пользователя перед изменением workflow или запуском деплоя.

### 2. Подготовь preflight

Направь пользователя по подготовке инфраструктуры. Дай конкретные команды или шаги, адаптированные под его сервер и layout:

- создать отдельного пользователя `deploy`, а не использовать `root`;
- настроить `~deploy/.ssh/authorized_keys` отдельным ключом;
- при необходимости добавить `deploy` в `docker` group, явно указав, что это root-equivalent доступ;
- убедиться, что директория приложения существует;
- убедиться, что `.env` уже находится на сервере и не будет перезаписан деплоем;
- настроить GitHub secrets:
  - `DEPLOY_HOST`;
  - `DEPLOY_USER`;
  - `DEPLOY_SSH_KEY`;
  - `DEPLOY_KNOWN_HOSTS`.

Не переходи к правкам workflow, пока пользователь не подтвердит, что preflight выполнен.

### 3. Внедри или обнови workflow

1. В режиме `setup` создай workflow на базе `references/workflow-template.yml`.
2. В режиме `debug` патчь существующий workflow минимально и целево.
3. Проверь обязательные свойства:
   - deploy на `main`;
   - `workflow_dispatch` с boolean `dry_run`;
   - strict SSH host checking;
   - precheck `.env`;
   - безопасная передача секретов;
   - health-check после деплоя;
   - rollback при провале health-check.
4. Обнови `README.md` или локальный deploy-документ:
   - список secrets;
   - manual fallback deploy;
   - rollback;
   - troubleshooting.

После правок остановись и дождись явного разрешения на `dry_run`.

### 4. Проведи dry-run

1. Запусти `workflow_dispatch` с `dry_run=true`.
2. Если `dry_run` падает, открой логи run и устрани причину по `references/troubleshooting.md`.
3. Повторяй `dry_run`, пока run не станет `success`.

Не запускай реальный deploy без явного подтверждения пользователя.

### 5. Проведи реальный deploy

1. Запусти реальный deploy (`dry_run=false` или push в `main`, если пользователь это подтвердил).
2. Проверь:
   - workflow завершился `success`;
   - `docker compose ps` показывает сервис в `running`;
   - health-check прошёл;
   - в логах приложения нет `Traceback`, `CRITICAL` или других явных fatal markers.
3. Если deploy неуспешен, примени rollback-процедуру из `references/troubleshooting.md`.

### 6. Заверши задачу

- Если проект ведёт `project_state.md` или локальный статус-документ, обнови его только в той части, где реально изменился deploy-контур.
- Если проект ведёт issue, review, notes или operational docs, обнови только релевантные артефакты.
- Покажи, что изменено, что проверено и какие ручные шаги остались.

## Guardrails

- Не редактируй workflow и не запускай deploy до подтверждения пользователя после сбора контекста.
- Не запускай `dry_run` без подтверждения пользователя.
- Не запускай реальный deploy без отдельного подтверждения пользователя после успешного `dry_run`.
- Не деплой через `root`.
- Не отключай `StrictHostKeyChecking`.
- Не печатай секреты в CI-логи.
- Не передавай секреты в аргументах SSH-команды.
- Передавай секреты в удалённый shell через stdin-скрипт.
- Не используй `git clean -fd` без `-e .env`.
- Помни, что `git clean -fd -e .env` не защищает другие untracked файлы.
- После git-операций на удалённом хосте выполняй `unset GH_TOKEN auth_header`.
- Учитывай, что `docker` group даёт root-equivalent доступ.

## Ресурсы

- Эталон workflow: `references/workflow-template.yml`
- Диагностика, recovery и rollback: `references/troubleshooting.md`

При падении `dry_run`, проблемах с SSH, `git`, health-check или rollback сначала смотри `references/troubleshooting.md`.

## Предпочтительный формат ответа

Выводи результат тремя блоками:

1. `Изменения` — какие файлы и шаги изменены.
2. `Проверки` — `dry_run`, реальный deploy и итоговые статусы.
3. `Ручные шаги` — что пользователь должен сделать сам, если что-то осталось.