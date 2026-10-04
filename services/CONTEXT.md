# Services — уровень приложений

## Purpose

Уровень всех исполняемых приложений платформы: 8 Node.js-бэкенд-сервисов и один
Flutter-клиент. Здесь же зафиксирован **общий шаблон архитектуры сервиса**, который
разделяют все Node-сервисы, — он описывается один раз на этом уровне и не повторяется
в дочерних документах.

## Abstraction level

Ниже корня, выше отдельного сервиса. Описывает семейство сервисов и контракты между ними,
не углубляясь во внутренности конкретного. Более конкретный уровень — дети (каждый сервис).

## Parent

[../CONTEXT.md](../CONTEXT.md) — корень владеет workspace-конфигурацией и запуском;
наш уровень владеет содержимым приложений.

## Children

- [auth/](auth/CONTEXT.md) — идентичность: анонимный вход, сессии, токены; gRPC Auth.
- [profile/](profile/CONTEXT.md) — профили игроков; gRPC Profile, Kafka user.created → profile.created.
- [gateway/](gateway/CONTEXT.md) — единственная точка входа снаружи: HTTP → gRPC-прокси.
- [orchestration/](orchestration/CONTEXT.md) — внутренний маршрутизатор HTTP-запросов
  поверх gRPC (Proxy).
- [streaming/](streaming/CONTEXT.md) — WebSocket-сервер для событий в реальном времени.
- [gusteau/](gusteau/CONTEXT.md) — кулинарный справочник: продукты, нутриенты, журнал еды; gRPC Gusteau.
- [remy/](remy/CONTEXT.md) — AI-бот; gRPC Health, клиент OpenRouter (не подключён).
- [speech-to-text/](speech-to-text/CONTEXT.md) — каркас сервиса распознавания речи (без реализации).
- [front/](front/CONTEXT.md) — Flutter-клиент (mobile/desktop/web), отдельная архитектура.
sdf
Без CONTEXT.md (не являются сервисами): `postgres/`, `pgbouncer/` — только конфиги
и данные для docker-compose (`pgbouncer.ini`, `userlist.txt`, каталог данных БД).

## Responsibility

- Содержимое `src/` каждого сервиса и его `__tests__`.
- Межсервисное взаимодействие: gRPC, Kafka, WebSocket, очереди pg-boss.
- Общий для Node-сервисов шаблон слоёв (см. ниже) и фактическое направление зависимостей
  между слоями.

## Non-responsibility

- Внутренние детали конкретного сервиса — описаны в его CONTEXT.md.
- Содержимое `@shared/*` (→ [../shared/](../shared/CONTEXT.md)).
- Определения proto-контрактов (→ [../proto/](../proto/CONTEXT.md)).
- Workspace-сборка, docker, CI (→ [../CONTEXT.md](../CONTEXT.md)).

## Dependencies

Уровень зависит от `shared/*` (workspace-пакеты `@shared/*`) и от `proto/`
(только на этапе codegen: `protoc --proto_path=../../proto/`).

## Dependency direction

- **Сервисы не импортируют друг друга.** Связь между ними только через протоколы:
  gRPC (по `SERVICE_*_URL`), Kafka (топики), WebSocket.
- Сервисы → `@shared/*`. Обратного направления не существует: shared не знает о сервисах.
- Сервисы → `proto/` строго на этапе генерации кода, не рантайм-зависимость.

### Шаблон Node-сервиса (общий для всех восьми)

Точка входа и wiring — везде одинаковы:

```
src/app.ts → src/composition/build-application.ts → src/application/application.ts
```

`app.ts` импортирует **только** `composition/`. `Application` стартует сервисы в порядке:
message (Kafka) → job (pg-boss) → db (Prisma) → grpc, плюс websocket в streaming;
graceful shutdown по SIGINT/SIGTERM.

Слои:

| Слой | Содержимое |
|---|---|
| `contracts/` | только сгенерированный ts-proto код (`proto/*`, `common/*`) |
| `domain/` | сущности, value-objects, интерфейсы репозиториев; не импортирует ничего кроме domain и node |
| `application/` | `application.ts` (lifecycle), `use-cases/*`, `ports/*` |
| `infrastructure/` | адаптеры: `grpc/` (сервер, хендлеры, клиенты, мапперы), `http/`, `db/` (Prisma), `auth/`, `message/` (Kafka), `job/` (pg-boss), плюс уникальные подсистемы конкретного сервиса |
| `composition/` | единственная точка DI: собирает сервисы, репозитории, use-cases, хендлеры |
| `utils/` | мелкие помощники (`sleep.ts`) |

**Фактическое направление зависимостей** (проверено по импортам, не идеальное —
зафиксировано как есть):

- `domain` → только `domain` (+ node). Чистый.
- `application` → `domain`, `contracts`, **и конкретные классы `infrastructure`**
  (use-cases импортируют `DbService`, `JobService`, `MessageService`, Prisma-репозитории,
  gRPC-клиенты, proto-мапперы, `app/config`).
- `infrastructure` → `application` (гRPC/HTTP-хендлеры и Kafka-консьюмеры вызывают use-cases),
  `domain` (репозитории `implements` доменные интерфейсы), `contracts`.
- ⇒ связь `application ↔ infrastructure` **взаимная**. Это фактическое состояние.
- `composition` → `application` + `infrastructure`; импортируется только из `app.ts`.
- Dependency inversion реализован фрагментарно: порт `application/ports/token-service.port.ts`
  реализуется в `infrastructure/auth/service.ts`; интерфейсы репозиториев объявлены в `domain/`
  и реализуются Prisma-репозиториями. Остальные зависимости — конкретные классы.

### Межсервисная топология (фактическая)

- `front` → HTTP `gateway:9090` и WebSocket `streaming:8080`.
- `gateway` → gRPC `orchestration.Proxy` (прокси всего HTTP-трафика); плюс health-фан-аут
  по всем `SERVICE_*_URL`.
- `orchestration` → gRPC клиенты `auth` и `profile` (через `@shared/grpc-client-manager`).
- `streaming` → gRPC `profile` (проверка профиля по токену).
- Kafka: `auth` produce `user.created` → `profile` consume; `profile` produce
  `profile.created`; `streaming` produce `bot.connecting-request`. Отправка в Kafka идёт
  через pg-boss → Kafka-мост (`@shared/pg-boss-manager`), а не напрямую.
- Каждый сервис регистрирует gRPC `Health` (`proto/common/health.proto`) на `:50051`.

## Entry points

Снаружи: `gateway` (HTTP 9090) и `streaming` (WebSocket 8080). Прочие сервисы доступны
только внутри сети (gRPC 50051).

## Structure

```
services/
├── <svc>/                 # git-submodule отдельного сервиса
│   ├── src/               # слои по шаблону выше
│   ├── __tests__/         # тесты (в части сервисов; см. jest testMatch)
│   ├── package.json       # deps: @shared/* workspace:*, prisma, kafkajs, pg-boss …
│   ├── Dockerfile         # multi-stage: base → deps → dev | build → prod + predeploy
│   ├── .env.dist          # шаблон env (compose копирует в .env)
│   └── jest.config.js, tsconfig*.json
├── front/                 # Flutter: lib/, web/, android/, ios/ … (см. front/CONTEXT.md)
├── postgres/, pgbouncer/  # конфиги инфраструктуры, не сервисы
```

## Architectural rules

- Новый use-case → `application/use-cases/<area>/`, он может импортировать domain,
  contracts и конкретный infrastructure (так принято фактически); регистрация —
  в `composition/build-application.ts` и в gRPC-хендлере/контроллере.
- Новый репозиторий → интерфейс в `domain/<area>/`, реализация в
  `infrastructure/db/repositories/`, маппер в `infrastructure/db/mappers/`.
- Новый RPC → правка `.proto` в `proto/` (отдельный submodule!) → `pnpm <svc> proto:generate`
  → хендлер в `infrastructure/grpc/handlers/`.
- Тесты: два фактических стиля — `*.spec.ts` рядом с кодом (auth, gateway, profile;
  `testMatch: ['**/*.spec.ts']`) и `__tests__/**/*.test.ts` (остальные пять; в
  orchestration, streaming тестов нет, а в remy/gusteau/speech-to-text лежит
  нерабочая копия старого battle-теста).
- Compose монтирует `src`, `__tests__`, `package.json` и `../../shared` в dev-контейнер;
  зависимость сервиса от правках shared подхватывается без пересборки.

## Important files

- `docker-compose.yml.dist` и `.env.dist` в каждом сервисе — его env-поверхность.
- `make/parameters.mk` (на уровне корня) — списки сервисов для миграций/сборок.

## Navigation

- Нужен конкретный сервис → его CONTEXT.md; после этого — `src/` этого сервиса
  по шаблону слоёв выше.
- Задача в общей библиотеке → [../shared/](../shared/CONTEXT.md).
- Задача в контракте → [../proto/](../proto/CONTEXT.md).
