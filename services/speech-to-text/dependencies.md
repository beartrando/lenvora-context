# Dependencies

Внутри монорепозитория:

- [proto](../../proto/purpose.md) — контракт `speech-to-text`, `common`.
- [shared](../../shared/purpose.md): `logger`, `errors`, `kafka-manager`, `pg-boss-manager`.
- Исходящих gRPC-клиентов к сервисам нет; входящий поток открывает [streaming](../streaming/purpose.md).

Внешние пакеты: `@grpc/grpc-js`, `ts-proto`, `@prisma/client`, `kafkajs`, `pg-boss`, `jsonwebtoken`, `ws`, `cookie-parser`, `cors`. SDK распознавания речи (OpenAI/Whisper и аналоги) в `package.json` отсутствует.

Инфраструктура: PostgreSQL через pgbouncer (очереди pg-boss), Kafka (`KAFKA_BROKERS`), gRPC-порт 50051. `MESSAGE_PRODUCER_EVENT_FIRED` и `PG_BOSS_WEBSOCKET_SEND_EVENT` объявлены в `.env.dist`, но конфигами тем не читаются.
