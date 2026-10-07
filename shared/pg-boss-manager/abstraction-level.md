# Abstraction level

Обёртка над очередью задач `pg-boss` (PostgreSQL) и мостом в Kafka: скрывает реконнекты, ready-семантику, восстановление воркеров и транзакционную постановку задач.

Ниже — `pg-boss`, [kafka-manager](../kafka-manager/purpose.md), [logger](../logger/purpose.md); выше — `services/*/src/infrastructure/job/service.ts` (JobService), который владеет запуском, конфигурацией и обработчиками задач.
