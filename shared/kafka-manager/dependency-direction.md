# Dependency direction

- Зависит от: `@shared/logger` (логирование admin/producer/consumer), `kafkajs`; от `services/*` не зависит.
- Зависятся от неё (проверено grep'ом по `services/*`):
  - 7 сервисов — все, кроме `gateway`: `services/auth`, `services/profile`, `services/remy`, `services/gusteau`, `services/orchestration`, `services/streaming`, `services/speech-to-text`;
  - импорты: `createConsumer`, `createProducer`, `KafkaConfig`, `KafkaProducer` в `src/infrastructure/message/service.ts`; `KafkaConfig` в `src/infrastructure/job/service.ts`;
  - [pg-boss-manager](../pg-boss-manager/purpose.md) — `createProducer` и `KafkaConfig` для моста pg-boss → Kafka.
- Направление: `logger` → `kafka-manager` → `services` и `pg-boss-manager`.
