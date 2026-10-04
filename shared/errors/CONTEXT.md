# @shared/errors — словарь доменных ошибок

## Purpose

Иерархия ошибок приложения: абстрактный `DomainError` с proto-кодом и 9 конкретных
классов, общих для всех сервисов и маппящихся в `common.error.ErrorObject`.

## Abstraction level

Лист дерева: нижний уровень [shared/](../CONTEXT.md).

## Parent

[../../shared/CONTEXT.md](../../shared/CONTEXT.md) — описывает место пакета среди
`@shared/*` и разрешённые направления зависимостей.

## Children

Нет. `src/contracts/proto/common/error.ts` — сгенерированный файл, не архитектурный узел.

## Responsibility

- `DomainError` (абстрактный, `readonly code: ErrorCode`) и наследники:
  `AccessDeniedError`, `InvalidAuthorizationError`, `InvalidSessionError`,
  `NotFoundError`, `ValidationError`, `TokenExpiredError`, `UnknownError`,
  `ServiceUnavailableError`, `InvalidResponseError`.
- Свой сгенерированный контракт `ErrorCode` (из `proto/common/error.proto`).

## Non-responsibility

- Логированием (→ [../logger/](../logger/CONTEXT.md)), маппингом ошибок в gRPC-ответы —
  это делают хендлеры сервисов (`infrastructure/grpc/handlers/callback.error.ts`),
  транспортных ошибок Kafka/pg-boss.

## Dependencies

`@bufbuild/protobuf` (runtime для сгенерированного кода). На codegen — `proto/common/error.proto`.

## Dependency direction

Только вниз к proto. Пакет не зависит от других shared и от сервисов; сервисы
импортируют его как `@shared/errors`.

## Entry points

`src/index.ts` — barrel, экспортирующий все классы ошибок.

## Structure

```
shared/errors/src/
├── index.ts
├── errors/{domain,not-found,token-expired,unknown,invalid-authorization,
│           invalid-session,access-denied,invalid-response,
│           validation-failed,service-unavailable}.error.ts
└── contracts/proto/common/error.ts   # protoc-gen-ts_proto, DO NOT EDIT
```

## Architectural rules

- Добавление нового типа ошибки: сначала `ErrorCode` в `proto/common/error.proto`
  (сабмодуль proto) → `pnpm --filter @shared/errors proto:generate` → новый класс
  → экспорт в `src/index.ts` → маппинг в хендлерах сервисов.
- Генерация выполняется и в Docker-стадии `deps` каждого сервиса, поэтому классы
  ошибок доступны сервисам без отдельной сборки пакета.

## Important files

- `src/errors/domain.error.ts` — базовый класс.
- `src/index.ts` — публичный API.

## Navigation

- Коды ошибок и их proto-источник → [../../proto/](../../proto/CONTEXT.md).
