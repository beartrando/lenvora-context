# Structure

```
shared/pg-boss-manager/src/
├── index.ts               # синглтон
├── types.ts               # PgBossConfig, обработчики
├── pg-boss-manager.ts     # PgBossManager: connect/start/stop/cleanup, sub-менеджеры
└── internal/
    ├── queue-manager.ts   # enqueue, enqueueTx (raw SQL в pgboss.job)
    ├── worker-manager.ts  # реестр воркеров + restore() после reconnect (самый большой)
    ├── kafka-bridge.ts    # KafkaBridge: pg-boss → Kafka
    ├── interfaces.ts      # BossProvider, SqlExecutor
    ├── types.ts, utils.ts
```
