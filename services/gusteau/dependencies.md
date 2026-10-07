# Dependencies

Внутри монорепозитория:

- [proto](../../proto/purpose.md) — контракт `gusteau`, `common`.
- [shared](../../shared/purpose.md): `logger`, `errors`, `kafka-manager`, `pg-boss-manager`.
- gRPC-клиентов к другим сервисам нет (каталога `infrastructure/grpc/clients` не существует).

Внешние пакеты: `@grpc/grpc-js`, `ts-proto`, `@prisma/client`, `kafkajs`, `pg-boss`, `jsonwebtoken`, `ws`, `cookie-parser`, `cors`.

Инфраструктура: PostgreSQL через pgbouncer (`gusteau` в имени БД), прямой доступ `DATABASE_DIRECT_URL` для миграций, Kafka (брокер из `KAFKA_BROKERS`), pg-boss. `MESSAGE_PRODUCER_EVENT_FIRED` и `PG_BOSS_WEBSOCKET_SEND_EVENT` объявлены в `.env.dist`, но кодом конфигов тем не читаются.
