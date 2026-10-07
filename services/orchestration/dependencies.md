# Dependencies

Код (`services/orchestration/package.json`):

- `@shared/grpc-client-manager` — переиспользование gRPC-каналов (клиенты auth/profile/gusteau); `@shared/logger`, `@shared/errors` (`ServiceUnavailableError`, `InvalidResponseError`) — [shared](../../shared/purpose.md).
- `@shared/kafka-manager`, `@shared/pg-boss-manager`, `pg-boss`, `kafkajs` — поднимаются в lifecycle и участвуют в health-проверке, но топиков не объявлено.
- `@prisma/client` + `prisma` (dev) — подключение к БД `orchestration` (схема без моделей).
- `jsonwebtoken` — верификация access-токена (`ACCESS_SECRET`); `bcrypt` объявлен, в коде не используется.
- `ioredis` — используется только модулем `infrastructure/cache`, который в composition не подключён (env `REDIS_HOST/PORT/PASSWORD`).
- `@grpc/grpc-js`, `@bufbuild/protobuf`, `ts-proto` (dev) — gRPC и генерация из [proto](../../proto/purpose.md) (`orchestration.proto`, `auth.proto`, `profile.proto`, `gusteau.proto`, `common/*`).
- jest/ts-jest (dev) — тестов в коде нет; nodemon/ts-node/typescript — dev-запуск.

Внешние адреса: `SERVICE_AUTH_URL=auth:50051`, `SERVICE_PROFILE_URL=profile:50051`, `SERVICE_GUSTEAU_URL=gusteau:50051`, postgres (pgbouncer:6432 и :5432), kafka:9092.
