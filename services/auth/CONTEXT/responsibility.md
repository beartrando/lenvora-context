# Responsibility

- gRPC-сервер `Auth` (`anonymousSignIn`, `get`, `logout`, `refreshTokens`) + `Health`.
- JWT `AuthService` (access/refresh), интерфейс — `application/ports/token-service.port.ts`.
- Prisma-модели `User`, `Session`, `Device` (БД `auth`).
- Kafka: produce `user.created` (событие уходит из use-case через pg-boss-мост).
- Health-хендлер: агрегирует db/kafka/job.
