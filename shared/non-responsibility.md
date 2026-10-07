# Non-responsibility

- Бизнес-логика, сценарии и домен — [services](../services/purpose.md).
- HTTP/gRPC-серверы, порты, DI-композиция, чтение конфигурации сервиса — в потребителях; библиотеки только предоставляют API.
- Управление инфраструктурой (Kafka-брокер, PostgreSQL, pgbouncer, docker-compose) — корень репозитория.
- proto-контракты и кодоген gRPC-клиентов/серверов — уровень [proto](../proto/purpose.md); `shared/errors` лишь генерирует из контракта свою копию.
- Тесты — в `services/*`; в `shared/` тестов и тестовых конфигураций нет.
- Фронтенд — `services/front` не использует `@shared/*`.
