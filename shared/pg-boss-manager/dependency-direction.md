# Dependency direction

- Зависит от: `@shared/logger` (логирование жизненного цикла и воркеров), `@shared/kafka-manager` (`createProducer`, `KafkaConfig` — мост в Kafka), `pg-boss`; от `services/*` не зависит.
- Зависятся от неё (проверено grep'ом по `services/*`): 7 сервисов — все, кроме `gateway`: `services/auth`, `services/profile`, `services/remy`, `services/gusteau`, `services/orchestration`, `services/streaming`, `services/speech-to-text`; импорты в `src/infrastructure/job/service.ts` (`pgBossManager`, типы `PgBossConfig`, `WorkerHandler`, `SqlExecutor`).
- Направление: `logger` → `kafka-manager` → `pg-boss-manager` → `services`; обратных связей нет, `services` библиотека не импортирует.
