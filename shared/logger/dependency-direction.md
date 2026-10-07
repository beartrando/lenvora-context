# Dependency direction

- Зависит от: ничего — ни от одной `@shared/*`, ни от `services/*`, ни от внешних пакетов. Нижний слой [../](../purpose.md).
- Зависятся от неё (проверено grep'ом по `services/*`):
  - все 8 Node-сервисов: `services/auth`, `services/gateway`, `services/profile`, `services/remy`, `services/gusteau`, `services/orchestration`, `services/streaming`, `services/speech-to-text` — обычно `src/application/application.ts`, `src/infrastructure/{grpc,job,message}/service.ts`;
  - библиотеки: [kafka-manager](../kafka-manager/purpose.md) (`admin`, `producer`, `consumer`), [grpc-client-manager](../grpc-client-manager/purpose.md), [pg-boss-manager](../pg-boss-manager/purpose.md).
- Направление: только входящие зависимости, исходящих нет.
