# Dependencies

Код (`services/profile/package.json`):

- `@shared/kafka-manager`, `@shared/pg-boss-manager`, `@shared/logger`, `@shared/errors` — workspace-пакеты ([shared](../../shared/purpose.md)); `@shared/grpc-client-manager` объявлен, но в коде profile не используется (клиентских gRPC-вызовов нет).
- `@prisma/client` + `prisma` (dev) — PostgreSQL, схема `src/infrastructure/db/prisma/schema.prisma`.
- `kafkajs`, `pg-boss` — consumer `user.created` и producer `profile.created` (через bridge).
- `jsonwebtoken` — проверка access-токена (`ACCESS_SECRET`).
- `@grpc/grpc-js`, `@bufbuild/protobuf`, `ts-proto` (dev) — gRPC и генерация из [proto](../../proto/purpose.md) (`profile.proto`, `auth.proto`, `common/*`).
- jest/ts-jest/supertest (dev) — тесты (`supertest` в коде не применяется).

Инфраструктура: postgres через pgbouncer (`…:6432/profile`) и напрямую (`…:5432/profile`), kafka (`kafka:9092`).

Внешние вызовы: нет — profile только принимает gRPC и читает/пишет БД и Kafka.
