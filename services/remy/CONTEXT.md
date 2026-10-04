# Remy — сервис AI-бота

## Purpose

Сервис AI-обработки входящих запросов: gRPC `Process` принимает текст, вызывает grpc orchestration и управляет приложением0
в `Process` стоит заглушка (эхо), реальный вызов AI не подключён.

## Abstraction level

Конкретный сервис внутри уровня [services/](../CONTEXT.md). Слои — по общему шаблону родителя.

## Parent

[../CONTEXT.md](../CONTEXT.md).

## Children

Нет дочерних CONTEXT.md.

## Responsibility

- gRPC-сервер на `:50051` с двумя сервисами: `Remy.Process` и `Health` (check/status/livez/readyz).
- `src/infrastructure/ai/` — `AiClient` интерфейс, `OpenRouterClient` (SDK `openai`),
  конфиг `OPENROUTER_API_KEY` / `AI_BASE_URL` / `AI_MODEL`. К wiring пока не подключён.
- Каркас приложения: lifecycle (`application.ts`: message → job → grpc),
  Prisma-подключение (моделей в схеме нет), auth, message/job-инфраструктура.

## Non-responsibility

- источник данных → speech-to-text.

## Dependencies

`@shared/{logger,errors,kafka-manager,pg-boss-manager}`, `openai ^5`,
`@prisma/client` (клиент сгенерирован, моделей в schema.prisma нет), `ws`, `jsonwebtoken`.
`grpc-client-manager` не заявлен. Контракты — `src/contracts/proto/` (генерируются, в git не попадают).

## Dependency direction

Как в родителе. AI-клиент пока не участвует в импортах вне `infrastructure/ai/`;
домен `user/profile/session/common` используется только `auth/service.ts` и
`application/ports/token-service.port.ts` (AuthService никуда не заведён).

## Entry points

- gRPC `:50051`: `remy.Remy/Process`, `Health/check|status|livez|readyz`.
- `src/app.ts` → `composition/build-application.ts` → `Application.start()`.
- Вызывает `Process` пока никто; gateway знает remy только через
  `SERVICE_REMY_URL` для health-агрегата.

## Structure

```
src/
├── app.ts
├── composition/build-application.ts     # wiring: job → message → health → process → grpc
├── application/
│   ├── application.ts                   # lifecycle: message → job → grpc
│   ├── ports/token-service.port.ts
│   └── use-cases/
│       ├── health/{health,status,livez,readyz}.ts   # восстановлены после «remove redundant code»
│       └── process/process.ts           # ЗАГЛУШКА: эхо + VALIDATION_FAILED на пустой текст
├── domain/                              # VO/сущности без use-cases: common, profile, session, user
├── infrastructure/
│   ├── ai/                              # OpenRouterClient, конфиги, интерфейсы — НЕ подключён
│   ├── app/, auth/                      # AuthService нигде не создаётся
│   ├── db/                              # DbService + пустая schema.prisma (моделей нет),
│   │                                    # seed.ts пуст; слой по решению не трогаем
│   ├── grpc/                            # service.ts: регистрация RemyService + Health
│   │   └── handlers/{remy,health,callback.error}
│   ├── job/, message/                   # продюсеры/консьюмеры пустые
│   └── utils/sleep.ts
└── contracts/proto/                     # сгенерированные ts-proto (gitignored)
```

## Architectural rules

- Контракт — источник истины в [../../proto/remy.proto](../../proto/remy.proto):
  `rpc Process(RemyRequest{text}) returns (RemyResponse{oneof: text|error})`.
  Правки контракта требуют `pnpm --filter remy proto:generate`.
- Порядок итерации по контракту: proto → `proto:generate` → use-case → handler →
  регистрация в `grpc/service.ts` → wiring в `composition` (по шаблону gusteau).
- `tsc --noEmit` и `pnpm --filter remy build` — зелёные (0 ошибок, проверено).
  ВНИМАНИЕ: раньше синтаксическая ошибка в одном файле (`ai/service.ts`) маскировала
  все остальные ошибки диагностики — при подозрительно чистом выводе проверять осознанно.
- Тесты — колокейтед, `*.spec.ts` рядом с кодом: `src/application/use-cases/process/
  process.spec.ts` (юнит, без зависимостей) и `src/infrastructure/grpc/handlers/
  remy.handler.spec.ts` (реальный gRPC на localhost:50051). Полный набор — изнутри
  контейнера: `docker compose exec -T -w /usr/src/app/services/remy remy pnpm test`
  (порт 50051 на хост не опубликован, функциональный спека с хоста не подключится);
  с хоста работает только юнит: `pnpm --filter remy test -- src/application/use-cases/`.
- env: `OPENROUTER_API_KEY` (пуст в .env/.env.dist), `AI_BASE_URL`, `AI_MODEL` — заданы с дефолтами.
- `CONTEXT.md` этого сервиса игнорируется git (`.gitignore`: `**/CONTEXT.md`).
- `seed`-скрипт в package.json указывает на несуществующий путь и `tsx` не в зависимостях (факт, не трогали).

## Git workflow

- Коммиты делаем в ветку `feature` (не в `dev` напрямую).
- Пушим в `origin`: `git push origin feature`.
- Пул-реквест через `gh` из `feature` в `dev`:
  `gh pr create --base dev --head feature --title "..." --body "..."`.

## Important files

- `proto/remy.proto` — контракт (правится в отдельном репо proto).
- `src/application/use-cases/process/process.ts` — точка подключения реального AI вместо эхо.
- `src/infrastructure/ai/clients/openrouter.ts` — единственный AI-клиент в проекте.
- `src/composition/build-application.ts` — единственная точка wiring.

## Navigation

- Шаблон слоёв и правила зависимостей → [../CONTEXT.md](../CONTEXT.md).
