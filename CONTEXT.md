# Lenvora Infra — корень монорепозитория

## Purpose

Корневой уровень монорепозитория продуктовой платформы Lenvora: сборка, запуск и развёртывание
всех приложений. Содержит только workspace-конфигурацию и инструменты оркестрации —
собственного кода приложений на этом уровне нет.

## Abstraction level

Высший уровень дерева. Описывает платформу целиком (сервисы + клиент + контракты + shared +
деплой), не углубляясь ни в один конкретный сервис. Более конкретные уровни — его дети.

## Parent

Нет — это корень.

## Children

- [services/](services/CONTEXT.md) — приложения: 8 Node-бэкенд-сервисов, Flutter-клиент,
  конфиги инфраструктуры.
- [shared/](shared/CONTEXT.md) — библиотеки `@shared/*`, используемые сервисами
  (logger, errors, kafka-manager, grpc-client-manager, pg-boss-manager).
- [proto/](proto/CONTEXT.md) — gRPC/protobuf-контракты (git-submodule), источник codegen'а.

Директории без собственного CONTEXT.md: `make/` (фрагменты Makefile), `.github/`
(workflows CI/CD) — описаны ниже как инструменты корня.

## Responsibility

- Pnpm workspace (`pnpm-workspace.yaml`: пакеты `services/*` и `shared/*`) и turbo-таски
  (`turbo.json`: `build`, `dev`).
- Общая TypeScript-база (`tsconfig.base.json`, без path-алиасов — `@shared/*` резолвится
  через workspace-линки `workspace:*`).
- Локальная разработка: `Makefile` + `make/*.mk` (`make/parameters.mk` — списки сервисов:
  `NODE_SERVICES`, `PRISMA_SERVICES`, `FLUTTER_SERVICES`), `docker-compose.yml`
  (рабочая копия, генерируется из `docker-compose.yml.dist`).
- Развёртывание: `docker-compose.yml.deploy.dist` (GHCR-образы, теги из `.env.dist`)
  и `.github/workflows/` (build → GHCR → деплой по SSH поверх docker-compose).
- Git-структура: сервисы и `proto/` — submodules (`.gitmodules`), `shared/` — обычная
  часть этого репозитория.

## Non-responsibility

- Бизнес-логика и внутренняя архитектура сервисов (→ `services/`, конкретный сервис).
- Содержимое shared-библиотек (→ `shared/`).
- Определения RPC/сообщений (→ `proto/`).
- Схемы БД приложений — лежат внутри сервисов, не здесь.

## Dependencies

Корень не импортирует код приложений. Потребители корневых конфигов — дочерние пакеты
(наследуют `tsconfig.base.json`, читают таски turbo) и инструменты (protoc, pnpm, docker).

## Dependency direction

Зависимости идут только сверху вниз: корень → пакеты. Пакеты не должны импортировать
корневые файлы, кроме конфигов сборки (`tsconfig.base.json`, `turbo.json`).

## Entry points

- `make install` — submodule'ы, env-файлы, codegen proto, `make up`.
- `make up` / `docker compose up -d` — локальный запуск всех сервисов.
- `pnpm build` (turbo) — сборка всех пакетов.
- `make test` — тесты всех node-сервисов в контейнерах.

## Structure

```
├── services/            # приложения (см. services/CONTEXT.md)
├── shared/              # библиотеки @shared/* (см. shared/CONTEXT.md)
├── proto/               # контракты (git-submodule, см. proto/CONTEXT.md)
├── make/                # фрагменты Makefile: parameters/kafka/front/terminal
├── .github/workflows/   # build-and-push, deploy-docker-compose, deploy-gke,
│                        # deploy-ssh-k8s (только manual), cleanup-registry, prune
├── Makefile             # install, up, reset, migrate, proto-generate, test, git-* …
├── docker-compose.yml   # dev-топология (рабочая копия; не в git)
├── docker-compose.yml.dist / .deploy.dist   # шаблоны dev и deploy
├── pnpm-workspace.yaml, turbo.json, tsconfig.base.json
└── .env.dist            # теги GHCR-образов для deploy-compose
```

## Architectural facts

- **Git-submodules**: `services/{auth,gateway,profile,remy,gusteau,front,orchestration,
  streaming,speech-to-text}` и `proto/` — отдельные репозитории; правки в них коммитятся
  в свои репозитории (`make git-commit-all`).
- **Топология dev-запуска** (сеть `backend`): gateway публикует `9090`, kafka `9092`,
  postgres `5432`; остальные сервисы внутри сети. Все сервисы зависят от `pgbouncer`
  (а auth/profile/orchestration/gusteau/streaming/speech-to-text — ещё от `kafka`).
- **Пересборка Docker-образов** (`docker compose build`) не выполняется без явного
  указания пользователя: dev-контейнеры получают код и конфиги через volume-монтирования
  docker-compose (`src`, `package.json`, jest-конфиги remy, `shared`) и пересоздаются
  через `docker compose up -d`.
- **Деплой**: активен путь docker-compose поверх SSH (`deploy-docker-compose.yml`,
  триггерится из `build-and-push.yml`). Kubernetes-манифестов в репозитории больше нет
  (директория `k8s/` удалена); workflows `deploy-ssh-k8s.yml` (manual) и `deploy-gke.yml`
  сохранены как legacy. `README.md` описывает старый план K3s — это план, не факт.
- **Codegen**: proto генерируется отдельно для каждого пакета (см. `proto/CONTEXT.md`),
  не централизованно.

## Important files

- `pnpm-workspace.yaml` — что является пакетом.
- `turbo.json` — таски сборки.
- `Makefile`, `make/parameters.mk` — списки сервисов и команды разработчика.
- `docker-compose.yml.dist`, `docker-compose.yml.deploy.dist` — топология запуска.
- `.gitmodules` — какие директории являются подпроектами.

## Navigation

- Задача в коде бэкенда → [services/](services/CONTEXT.md) → нужный сервис.
- Задача в общей библиотеке → [shared/](shared/CONTEXT.md).
- Задача в контрактах RPC → [proto/](proto/CONTEXT.md).
- Задача по сборке/деплою → `Makefile`, `.github/workflows/`, `docker-compose*.yml`.
