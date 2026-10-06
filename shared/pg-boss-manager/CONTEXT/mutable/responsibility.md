# Responsibility

- `pgBossManager` — синглтон (default export) с полями `workers`, `queue`, `kafka`:
  `start(config)` (бесконечный reconnect-цикл: connect → heartbeat 1s → cleanup →
  пауза 3s), `stop()`, `getBoss()`, `waitUntilReady()`, `check()`.
- `queue.enqueue(topic, data)` и `enqueueTx(topic, data, tx)` — сырой INSERT в
  `pgboss.job` внутри вызывающей Prisma-транзакции (атомарность «данные + событие»).
- `workers.registerWorker/registerBatchWorker` — воркеры с отложенной привязкой
  (регистрация до connect сохраняется и восстанавливается после reconnect).
- `kafka.start(kafkaConfig, topic)` — создаёт Kafka-продюсер и batch-воркер (batch 10),
  пересылающий задания в топик Kafka.
