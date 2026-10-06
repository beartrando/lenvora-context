# Architectural rules

- Порядок старта в сервисах: `message.start()` → `job.start()` → `db.start()` →
  `grpc.start()`; мост Kafka запускается после успешного connect.
- Ключевой контракт для сервисов: имя очереди pg-boss == имя Kafka-топика
  (например `user.created`), поэтому `producers.config.ts` сервиса задаёт и топик
  Kafka, и очередь.
- `enqueueTx` обязателен для событий, которые должны уйти вместе с бизнес-данными
  в одной транзакции (пример: `auth` при анонимном входе).
- Пакетованные `package-lock.json` (npm) в каталогах `kafka-manager`,
  `grpc-client-manager`, `pg-boss-manager` — артефакты, проект использует pnpm.
