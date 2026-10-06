# Dependency direction

pg-boss-manager → kafka-manager → logger. Не зависит от сервисов и proto.
Потребители — `services/*/src/infrastructure/job/service.ts` (объединяет
`PgBossManager` + конфиги топиков и вызывает `initKafkaBridge`).
