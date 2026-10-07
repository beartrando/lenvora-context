# Children

- [services/](services/purpose.md) — 9 сервисов: 8 Node-бэкенд (gateway, auth, profile, remy, gusteau, orchestration, streaming, speech-to-text) + Flutter front; плюс конфиги `postgres/` и `pgbouncer/`.
- [shared/](shared/purpose.md) — общие библиотеки `@shared/*`: logger, errors, kafka-manager, grpc-client-manager, pg-boss-manager.
- [proto/](proto/purpose.md) — gRPC/protobuf-контракты (подмодуль, отдельный репозиторий).
- `make/`, `.github/` — без собственного контекста: инструменты корня, описаны в [responsibility](responsibility.md) и [architectural rules](architectural-rules.md).
