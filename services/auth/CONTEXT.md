# Auth — сервис идентичности

## Purpose

Анонимный вход в игру, создание пользователя/устройства/сессии, выпуск и обновление
JWT-токенов, выдача данных пользователя по RPC.

## Abstraction level

Конкретный сервис внутри уровня [services/](../CONTEXT.md). Ниже — свой `src/` по общему
шаблону слоёв (см. родительский документ, раздел «Шаблон Node-сервиса»).

## Parent

[../CONTEXT.md](../CONTEXT.md) — задаёт общий шаблон слоёв, направление зависимостей и
правила codegen. Повторять эти правила здесь не нужно.

## Children

Нет дочерних CONTEXT.md. Внутренняя структура — в `src/` по шаблону родителя:
`contracts/ domain/ application/ infrastructure/ composition/ utils/`.

## Responsibility

- gRPC-сервер `Auth` (`anonymousSignIn`, `get`, `logout`, `refreshTokens`) + `Health`.
- JWT `AuthService` (access/refresh), интерфейс — `application/ports/token-service.port.ts`.
- Prisma-модели `User`, `Session`, `Device` (БД `auth`).
- Kafka: produce `user.created` (событие уходит из use-case через pg-boss-мост).
- Health-хендлер: агрегирует db/kafka/job.

## Non-responsibility

- Профили (→ profile), маршрутизация (→ orchestration), вход через HTTP (→ gateway).
- Общие библиотеки, контракты proto, инфраструктура запуска.

## Dependencies

`@shared/{logger,errors,kafka-manager,pg-boss-manager}` (заявлен и `grpc-client-manager`,
но не импортируется), `@prisma/client`, `pg-boss`, `kafkajs`, `bcrypt`, `jsonwebtoken`.

## Dependency direction

Внутреннее направление — как в родителе (`domain` чистый, `application ↔ infrastructure`
взаимно). Сервис зависит от shared и от proto-контрактов (codegen); сам от него никто
не зависит, кроме вызовов по gRPC.

## Entry points

- gRPC `:50051` — `Auth` и `Health`.
- `src/app.ts` → `composition/build-application.ts`.
- Миграции/сиды: `pnpm migrate`, `pnpm seed`.

## Structure

- `src/application/use-cases/auth/` — `anonymous-sign-in`, `refresh-tokens`, `logout`, `get-user`;
  `use-cases/health/` — `health`, `readyz`.
- `src/infrastructure/grpc/` — `service.ts` (регистрация сервисов), `handlers/auth.handler.ts`,
  `helpers/auth.helper.ts` (Bearer из metadata), `mappers/user.proto.mapper.ts`.
- `src/infrastructure/db/` — `service.ts`, `repositories/prisma-{user,session,device}.repository.ts`,
  `prisma/schema.prisma`, `seed/`.
- `src/infrastructure/auth/`, `message/`, `job/` — JWT, Kafka, pg-boss.
- Тесты: `*.spec.ts` рядом с кодом (use-cases + `auth.handler.spec.ts`).

## Architectural rules

- `logout` и `get` требуют access-token из gRPC metadata (`AuthHelper`);
  `refreshTokens` верифицирует refresh-token из тела запроса.
- `user.created` публикуется **внутри Prisma-транзакции** через `jobService.enqueue` —
  это не прямой Kafka-producer.
- env-секреты (`ACCESS_SECRET`, `REFRESH_SECRET`) обязательны: падение при старте, если не заданы.

## Git workflow

- Коммиты делаем в ветку `feature` (не в `dev` напрямую).
- Пушим в `origin`: `git push origin feature`.
- Пул-реквест через `gh` из `feature` в `dev`:
  `gh pr create --base dev --head feature --title "..." --body "..."`.

## Important files

- `src/composition/build-application.ts` — единственная точка wiring.
- `src/infrastructure/db/prisma/schema.prisma` — модели User/Session/Device.
- `src/infrastructure/grpc/handlers/auth.handler.ts` — все RPC.

## Navigation

- Изменение бизнес-логики входа → `src/application/use-cases/auth/`.
- Изменение JWT → `src/infrastructure/auth/` и `src/application/ports/token-service.port.ts`.
- Изменение RPC-поверхности → сначала [../../proto/](../../proto/CONTEXT.md), затем хендлер.
