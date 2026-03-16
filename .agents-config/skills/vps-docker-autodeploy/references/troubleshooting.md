# Troubleshooting Playbook

## `Permission denied (publickey,password)`

Причина:
- неверный `DEPLOY_SSH_KEY`;
- ключ не добавлен в `~deploy/.ssh/authorized_keys`;
- неверный `DEPLOY_USER`.

Проверки:

```bash
ssh -i ~/.ssh/<deploy-key> deploy@<host> 'whoami'
```

Проверь ограничения ключа в `authorized_keys`:
- `no-port-forwarding`
- `no-agent-forwarding`
- `no-X11-forwarding`
- `restrict` (если доступно в версии OpenSSH)
- `from="<trusted-ip-or-cidr>"` при возможности ограничить source IP

## `fatal: not a git repository`

Причина:
- на VPS в директории приложения нет `.git`.

Решение:
- в workflow реализовать bootstrap ветку:
  - clone во временную директорию,
  - перенести `.env`,
  - заменить рабочую директорию.

## `could not read Username for 'https://github.com'`

Причина:
- private repo, но на VPS нет git-авторизации.

Решение:
- использовать в workflow `GITHUB_TOKEN` текущего run для `git fetch/clone`
  через `http.<url>.extraheader`.

После git-операций очищать переменные:

```bash
unset GH_TOKEN auth_header
```

## `scheduler startup marker not found`

Причина:
- проверка смотрит неподходящее окно логов;
- контейнер уже давно работает и маркер старта вне текущего `--since`.

Решение:
- брать `startedAt` текущего контейнера через `docker inspect`;
- читать логи `--since "$startedAt"`;
- требовать marker только для свежезапущенного контейнера.

## Потенциальная утечка токена в SSH cmdline

Антипаттерн:

```bash
ssh ... "GH_TOKEN='...' bash -s"
```

Почему плохо:
- токен виден в аргументах процесса на удалённой машине.

Решение:
- передавать переменные в удалённый shell через stdin-скрипт.

## `git clean` удалил `.env`

Причина:
- использован `git clean -fd` без исключений.

Решение:

```bash
git clean -fd -e .env
```

Важно:
- `-e .env` защищает только `.env`.
- другие untracked файлы всё равно удаляются.
- не хранить на сервере вручную созданные артефакты в директории релиза.

## Push workflow не проходит (`workflow scope`)

Симптом:
- GitHub отклоняет push с изменением `.github/workflows/*` из-за токена без `workflow` scope.

Решение:
- использовать токен с `repo,workflow`;
- или пушить через SSH-ключ, привязанный к GitHub-аккаунту с правом на репозиторий.

## `docker group` риск

Симптом:
- попытка считать deploy-пользователя "низкопривилегированным", когда он в `docker` группе.

Факт:
- `docker` group фактически root-equivalent.

Рекомендация:
- считать deploy-ключ критичным секретом (как root credential),
- ротировать ключи,
- ограничивать источник доступа и аудитить использования.

## Deploy сломал production (Rollback)

Симптом:
- deploy workflow упал после `docker compose up`,
- сервис не отвечает штатно.

Шаги rollback:

1. Подключись на VPS и перейди в директорию приложения.
2. Определи предыдущий стабильный commit.
3. Выполни rollback до `PREV_COMMIT`.
4. Пересобери и подними сервис.
5. Проверь состояние контейнера и логи.
6. Зафиксируй инцидент в issue/notes.

Команды:

```bash
cd /opt/<app>
git fetch --prune origin
git log --oneline -n 20
# Выбери стабильный commit и подставь его вместо <stable-sha>
PREV_COMMIT="<stable-sha>"
git checkout --detach "$PREV_COMMIT"
docker compose up -d --build
docker compose ps
docker compose logs --tail=200 <service-name>
```

Возврат на `main` после исправления:

```bash
cd /opt/<app>
git checkout main
git reset --hard origin/main
docker compose up -d --build
```