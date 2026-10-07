# pg-boss-manager — менеджер очереди pg-boss: воркеры, задачи, мост в Kafka

`@shared/pg-boss-manager` — default-экспорт-синглтон `pgBossManager` класса `PgBossManager`: `start`/`stop`/`waitUntilReady` с автореконнектом к PostgreSQL (цикл жизни, повтор через 3000 мс).

Три рабочих члена: `workers` (регистрация и восстановление воркеров после reconnect), `queue` (`enqueue`, `enqueueTx`), `kafka` (мост pg-boss → Kafka через `createProducer` из [kafka-manager](../kafka-manager/purpose.md)).

Подключение — `process.env.DATABASE_URL`; конфиг `PgBossConfig { applicationName, max?, maintenanceIntervalSeconds? }` приходит от сервиса (env `PG_BOSS_*`).

Потребляется 7 сервисами (все, кроме gateway) через `infrastructure/job/service.ts`.
