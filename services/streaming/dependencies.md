# Dependencies

Внутри монорепозитория:

- [proto](../../proto/purpose.md) — контракты `streaming`, `speech-to-text`, `profile`, `auth`, `common`.
- gRPC-вызовы: [profile](../profile/purpose.md) (проверка профиля), [speech-to-text](../speech-to-text/purpose.md) (bidi-поток `Transcribe`). `SERVICE_AUTH_URL` объявлен в конфиге, но клиент auth не создаётся — JWT проверяется локально.
- [shared](../../shared/purpose.md): `logger`, `errors`, `grpc-client-manager`, `kafka-manager`, `pg-boss-manager`.

Внешние пакеты: `ws`, `@grpc/grpc-js`, `ts-proto`, `jsonwebtoken`, `kafkajs`, `pg-boss`, `cookie-parser`, `cors`, `ioredis` (объявлен, в `src` не используется).

Инфраструктура: PostgreSQL через pgbouncer (очереди pg-boss), Kafka (продюсер темы `bot.connecting-request`, health-проверка пишет в `healthcheck`), Redis (в docker-compose `streaming` зависит от `redis`).
