# Architectural rules

- Один синглтон `pgBossManager` на процесс; сервис вызывает `start()` без await (`void`) и дальше ждёт `waitUntilReady()`.
- Внешний код работает только с публичными методами и членами `workers` / `queue` / `kafka`; `src/internal/*` не публикуется (кроме типа `SqlExecutor`).
- Внутренние менеджеры зависят от интерфейса `BossProvider`, а не от класса.
- Переподключение и восстановление воркеров происходят внутри библиотеки — сервис не перерегистрирует воркеров после сбоя.
- `enqueueTx` принимает только транзакцию, реализующую `$queryRawUnsafe` (Prisma-tx потребителя) — SQL вставки в `pgboss.job` зашит в библиотеке.
- `DATABASE_URL` читается библиотекой из окружения; `PgBossConfig` задаётся потребителем (конфиг в `services/*/src/infrastructure/job/`, env `PG_BOSS_MAX`, `PG_BOSS_MAINTENANCE_INTERVAL_SECONDS`, `PG_BOSS_APPLICATION_NAME`).
- Мост `kafka.start(config, topic)` запускается сервисом на каждый топик из его `producers.config`.
