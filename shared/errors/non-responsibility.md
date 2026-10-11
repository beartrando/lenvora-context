# Non-responsibility

- Логированием (→ [../logger/](../../logger**/CONTEXTpurpose.md)), маппингом ошибок в gRPC-ответы —
  это делают хендлеры сервисов (`infrastructure/grpc/handlers/callback.error.ts`),
  транспортных ошибок Kafka/pg-boss.
