# Dependencies

Внутри монорепозитория:

- [proto](../../proto/purpose.md) — контракт `remy`, `common`.
- [shared](../../shared/purpose.md): `logger`, `errors`, `kafka-manager`, `pg-boss-manager`.
- Дочерний узел [src/infrastructure/ai](src/infrastructure/ai/purpose.md) — SDK `openai` (OpenRouter-совместимый endpoint).
- gRPC-клиентов к другим сервисам нет; исходящих вызовов кроме AI не предусмотрено.

Инфраструктура: PostgreSQL через pgbouncer (очереди pg-boss, миграции Prisma), Kafka (брокер есть, тем нет), внешний HTTPS-эндпоинт AI (`AI_BASE_URL`, по умолчанию `https://openrouter.ai/api/v1`).

Env (`.env`, шаблон `.env.dist`): `GRPC_PORT`, `DATABASE_URL`, `DATABASE_DIRECT_URL`, `KAFKA_BROKERS`, `KAFKA_CLIENT_ID`, `MESSAGE_PRODUCER_EVENT_FIRED`, `PG_BOSS_*`, `ACCESS_SECRET`, `OPENROUTER_API_KEY`, `AI_BASE_URL`, `AI_MODEL`.
