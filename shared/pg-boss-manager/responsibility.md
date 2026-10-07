# Responsibility

- Жизненный цикл: `start(config)` — цикл `connect()` → опрос `sleep(1000)` → `cleanup()` → пауза 3000 мс → переподключение; `stop()` останавливает цикл; `void pgBossManager.start(...)` вызывается без await.
- `connect()`: `new PgBoss({ connectionString: process.env.DATABASE_URL, max (по умолч. 5), maintenanceIntervalSeconds (60), application_name })`, `boss.start()`, подписка на `error` → cleanup; после подключения — `workers.restore()`.
- Ready-семантика: `waitUntilReady()` ждёт первый успешный connect; `getBoss()` бросает, если не подключён; `tryGetBoss()` → `null`; `check()` → булево (health-проверка сервиса).
- `workers`: `registerWorker(topic, handler)` и `registerBatchWorker(topic, batchSize, handler)` — регистрация до подключения откладывается, после каждого reconnect'а `restore()` заново создаёт очередь (`boss.createQueue`, ошибка «уже существует» игнорируется) и вызывает `boss.work(...)` (batch — с `batchSize`).
- `queue`: `enqueue(topic, data)` → `boss.send` (пустой id — throw); `enqueueTx(topic, data, tx)` — `insert into pgboss.job(name, data) ... returning id` через `$queryRawUnsafe` в транзакции потребителя (атомарность бизнес-данных и задачи).
- `kafka.start(kafkaConfig, topic)`: создаёт producer и регистрирует batch-воркер (batchSize 10), который пересылает каждую задачу в Kafka-топик — мост pg-boss → Kafka.
