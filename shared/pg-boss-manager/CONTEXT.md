# @shared/pg-boss-manager — очереди, воркеры и мост в Kafka

## Purpose

Обёртка над `pg-boss` (очереди в PostgreSQL): жизненный цикл с переподключением,
постановка задач (в т. ч. внутри Prisma-транзакции), регистрация воркеров и
**мост pg-boss → Kafka**, через который сервисы фактически публикуют события.

## Abstraction level

Лист дерева: нижний уровень [shared/](../CONTEXT.md). Самый объёмный пакет уровня.

## Parent

[../../shared/CONTEXT.md](../../shared/CONTEXT.md).

## Children

Нет (пакет внутренне разбит на `src/internal/*` — это структура одного пакета).

## Responsibility

- `pgBossManager` — синглтон (default export) с полями `workers`, `queue`, `kafka`:
  `start(config)` (бесконечный reconnect-цикл: connect → heartbeat 1s → cleanup →
  пауза 3s), `stop()`, `getBoss()`, `waitUntilReady()`, `check()`.
- `queue.enqueue(topic, data)` и `enqueueTx(topic, data, tx)` — сырой INSERT в
  `pgboss.job` внутри вызывающей Prisma-транзакции (атомарность «данные + событие»).
- `workers.registerWorker/registerBatchWorker` — воркеры с отложенной привязкой
  (регистрация до connect сохраняется и восстанавливается после reconnect).
- `kafka.start(kafkaConfig, topic)` — создаёт Kafka-продюсер и batch-воркер (batch 10),
  пересылающий задания в топик Kafka.

## Non-responsibility

- Транспортного уровня Kafka (→ [../kafka-manager/](../kafka-manager/CONTEXT.md),
  откуда приходит `createProducer`), бизнес-содержимого задач (задачи кладут сервисы),
  Prisma-схем (сервисы).

## Dependencies

`pg-boss`, [../kafka-manager/](../kafka-manager/CONTEXT.md) (→ транзитивно
[../logger/](../logger/CONTEXT.md)). `DATABASE_URL` читается из env при connect.

## Dependency direction

pg-boss-manager → kafka-manager → logger. Не зависит от сервисов и proto.
Потребители — `services/*/src/infrastructure/job/service.ts` (объединяет
`PgBossManager` + конфиги топиков и вызывает `initKafkaBridge`).

## Entry points

`src/index.ts` — default export синглтона + типы `PgBossConfig`, `WorkerHandler<T>`,
`BatchWorkerHandler<T>`, `SqlExecutor`.

## Structure

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

## Architectural rules

- Порядок старта в сервисах: `message.start()` → `job.start()` → `db.start()` →
  `grpc.start()`; мост Kafka запускается после успешного connect.
- Ключевой контракт для сервисов: имя очереди pg-boss == имя Kafka-топика
  (например `user.created`), поэтому `producers.config.ts` сервиса задаёт и топик
  Kafka, и очередь.
- `enqueueTx` обязателен для событий, которые должны уйти вместе с бизнес-данными
  в одной транзакции (пример: `auth` при анонимном входе).
- Пакетованные `package-lock.json` (npm) в каталогах `kafka-manager`,
  `grpc-client-manager`, `pg-boss-manager` — артефакты, проект использует pnpm.

## Important files

- `src/pg-boss-manager.ts` — жизненный цикл.
- `src/internal/kafka-bridge.ts` — фактический механизм публикации событий.
- `src/README.md` — существует, но пуст.

## Navigation

- Где сервис регистрирует воркеры/топики → `services/<svc>/src/infrastructure/job/`
  и `.../message/configs/`.
