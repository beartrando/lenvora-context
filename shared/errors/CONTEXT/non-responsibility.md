# Non-responsibility

- Логированием (→ [../logger/](../../logger/CONTEXT/purpose.md)), маппингом ошибок в gRPC-ответы —
  это делают хендлеры сервисов (`infrastructure/grpc/handlers/callback.error.ts`),
  транспортных ошибок Kafka/pg-boss.
