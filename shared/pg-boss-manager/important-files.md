# Important files

- `src/index.ts` — синглтон и публичные типы.
- `src/pg-boss-manager.ts` — жизненный цикл: `connect`/`cleanup`, ready-промис, цикл реконнекта с паузой 3000 мс, `DATABASE_URL`.
- `src/internal/worker-manager.ts` — реестр воркеров, `ensureQueue`, `boss.work` (обычный и batch), `restore()` после reconnect.
- `src/internal/queue-manager.ts` — `enqueue` (`boss.send`) и `enqueueTx` (insert в `pgboss.job` в транзакции).
- `src/internal/kafka-bridge.ts` — мост: batch-воркер (10) → `producer.send(topic, job)`.
- `src/internal/interfaces.ts` — `BossProvider`, `SqlExecutor` (контракт для внутренних менеджеров и транзакций потребителя).
- `src/types.ts` — `PgBossConfig`, `WorkerHandler`, `BatchWorkerHandler`.
- `package.json`, `pnpm-workspace.yaml`, `README.md` — версии, fallback-оверрайды, порядок релиза.
- Потребители: `services/*/src/infrastructure/job/service.ts`, конфиг рядом (`job/config.ts`, `job/configs/job.config.ts` или `job/job.config.ts`).
