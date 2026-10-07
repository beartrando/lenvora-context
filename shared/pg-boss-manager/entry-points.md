# Entry points

`@shared/pg-boss-manager` → `exports["."]`:

- default export — синглтон `pgBossManager`;
- именованные типы: `PgBossConfig`, `WorkerHandler<T>`, `BatchWorkerHandler<T>`, `SqlExecutor` (`$queryRawUnsafe<T>(query, ...values)`).

API синглтона: `start(config)`, `stop()`, `waitUntilReady()`, `tryGetBoss()`, `getBoss()`, `check()`; члены `workers` (`registerWorker`, `registerBatchWorker`, `restore`), `queue` (`enqueue`, `enqueueTx`), `kafka` (`start(kafkaConfig, topic)`).

Тип `PgBoss` (из pg-boss) отдаётся `getBoss()`/`waitUntilReady()`; в сервисах напрямую вызывается `waitUntilReady()` (старт, `queue.enqueue*`), а статус очереди в health даёт `check()`.
