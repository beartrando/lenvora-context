# Profile — сервис профилей

## Purpose

Хранение и выдача игровых профилей (ник, уровень, рейтинг, опыт, достижения);
создание профиля при первом обращении.

## Abstraction level

Конкретный сервис внутри уровня [services/](../CONTEXT.md). Слои — по общему шаблону родителя.

## Parent

[../CONTEXT.md](../CONTEXT.md).

## Children

Нет дочерних CONTEXT.md.

## Responsibility

- gRPC-сервер `Profile` (`getOrCreate`, `get`, `getByUser`) + `Health`.
- Prisma-модели `Profile`, `ProfileAchievement`, `Achievement`, `ExperienceLevel` (БД `profile`).
- Kafka: consume `user.created` (→ `GetOrCreateUseCase`), produce `profile.created`
  (через pg-boss-мост).
- Верификация access-token: `getOrCreate` берёт `userId` из токена в metadata,
  а не из запроса.

## Non-responsibility

- Аутентификация и сессии (→ auth), маршрутизация (→ orchestration), события (→ streaming).

## Dependencies

`@shared/{logger,errors,kafka-manager,pg-boss-manager}`, `@prisma/client`, `pg-boss`,
`kafkajs`, `jsonwebtoken` (заявлен `grpc-client-manager`, но не импортируется).

## Dependency direction

Как в родителе. Единственный из сервисов, у которого есть Kafka-консьюмер,
зарегистрированный в `composition/build-application.ts` (`UserCreatedConsumer`).

## Entry points

- gRPC `:50051` — `Profile` и `Health`.
- Kafka-консьюмер `user.created` (группа `profile-service-user.created`).
- `src/app.ts`.

## Structure

- `src/application/use-cases/profile/` — `get-or-create`, `get`, `get-by-user`; `health/`.
- `src/domain/profile/` — `profile.entity.ts`, `profile.repository.ts` (интерфейс),
  `value-objects/` (profile-id, nickname, level, rating, experience).
- `src/infrastructure/db/repositories/prisma-profile.repository.ts`,
  `mappers/profile.db.mapper.ts`, `prisma/schema.prisma`.
- `src/infrastructure/message/consumers/user-created.ts` — входящее событие.
- `src/infrastructure/grpc/handlers/profile.handler.ts`, `helpers/auth.helper.ts`.
- Тесты `*.spec.ts`.

## Architectural rules

- `profile.created` публикуется без `await` (fire-and-forget) вне транзакции — факт.
- `GetOrCreateUseCase` — точка, где пересекаются Kafka-вход и Kafka-выход.

## Important files

- `src/application/use-cases/profile/get-or-create.ts` — ядро сервиса.
- `src/infrastructure/db/prisma/schema.prisma`.
- `.env.dist` — топики `MESSAGE_*_TOPIC_*`.

## Navigation

- Изменение событий → Kafka-конфиги `src/infrastructure/message/configs/`,
  контракт события → [../../proto/](../../proto/CONTEXT.md) (`auth.proto` `UserObject`).
