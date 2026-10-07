# Dependencies

Код (`services/auth/package.json`):

- `@shared/grpc-client-manager`, `@shared/kafka-manager`, `@shared/pg-boss-manager`, `@shared/logger`, `@shared/errors` — workspace-пакеты ([shared](../../shared/purpose.md)).
- `@prisma/client` + `prisma` (dev) — PostgreSQL; схема `src/infrastructure/db/prisma/schema.prisma`.
- `kafkajs`, `pg-boss` — событие `user.created` через pg-boss bridge.
- `jsonwebtoken` — JWT; `bcrypt` — зависимость (в текущем коде auth не используется, пароли не создаются).
- `@grpc/grpc-js`, `@bufbuild/protobuf`, `ts-proto` (dev) — gRPC и генерация контрактов из [proto](../../proto/purpose.md).
- jest/ts-jest/supertest (dev) — тесты; nodemon/ts-node/typescript — dev-запуск.

Инфраструктура: postgres через pgbouncer (`DATABASE_URL` …:6432/auth) и напрямую (`DATABASE_DIRECT_URL` …:5432/auth), kafka (`KAFKA_BROKERS=kafka:9092`).

Внешние вызовы: нет — auth никуда не ходит (кроме БД и Kafka).
