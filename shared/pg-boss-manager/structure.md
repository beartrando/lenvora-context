# Structure

```text
pg-boss-manager/
├── src/
│   ├── index.ts                  — синглтон new PgBossManager() + ре-экспорт типов
│   ├── pg-boss-manager.ts        — класс PgBossManager (connect/cleanup/цикл реконнекта)
│   ├── types.ts                  — PgBossConfig, WorkerHandler<T>, BatchWorkerHandler<T>
│   ├── README.md                 — (пустой файл в src/)
│   └── internal/
│       ├── interfaces.ts         — BossProvider, SqlExecutor
│       ├── worker-manager.ts     — реестр воркеров, attach/restore, ensureQueue
│       ├── queue-manager.ts      — enqueue / enqueueTx
│       ├── kafka-bridge.ts       — мост pg-boss → Kafka (batch-воркер + producer)
│       ├── types.ts              — InternalWorkerRegistration, InternalBatchWorkerRegistration
│       └── utils.ts              — sleep
├── dist/                         — tsc-выход (включая dist/internal/*)
├── package.json                  — deps: pg-boss, @shared/logger, @shared/kafka-manager
├── tsconfig.json, README.md
├── pnpm-workspace.yaml, pnpm-lock.yaml   — автономная установка вне монорепы
```

Синглтон создаётся при импорте (`const pgBossManager = new PgBossManager()`), подключение начинается только с `start()`.
