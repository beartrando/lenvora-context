# Architectural rules

- `logout` и `get` требуют access-token из gRPC metadata (`AuthHelper`);
  `refreshTokens` верифицирует refresh-token из тела запроса.
- `user.created` публикуется **внутри Prisma-транзакции** через `jobService.enqueue` —
  это не прямой Kafka-producer.
- env-секреты (`ACCESS_SECRET`, `REFRESH_SECRET`) обязательны: падение при старте, если не заданы.
