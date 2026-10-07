# Dependency direction

- auth → [shared](../../shared/purpose.md) (`logger`, `errors`, `kafka-manager`, `pg-boss-manager`, `grpc-client-manager`) и → [proto](../../proto/purpose.md) (сгенерированный `contracts/proto/auth.ts`).
- auth → PostgreSQL (своя БД `auth`) и → Kafka (producer топика `user.created`); обратно из Kafka не читает (consumers пуст).
- auth не импортирует код других сервисов и не вызывает их по сети.
- Вызывающие auth: [orchestration](../../services/orchestration/purpose.md) (gRPC-клиент `AuthClient`) и [gateway](../../services/gateway/purpose.md) (только health-агрегация по `SERVICE_AUTH_URL`).
- Обратная связь через Kafka: событие `user.created` уходит в [profile](../../services/profile/purpose.md) — auth не ждёт от него ответа.
- Зависимость по секрету: [profile](../../services/profile/purpose.md), [orchestration](../../services/orchestration/purpose.md) и [streaming](../../services/streaming/purpose.md) проверяют access-токен локально по общему `ACCESS_SECRET`, не вызывая auth.
