# Children

- [services/](../services/CONTEXT/purpose.md) — приложения: 8 Node-бэкенд-сервисов, Flutter-клиент,
  конфиги инфраструктуры.
- [shared/](../shared/CONTEXT/purpose.md) — библиотеки `@shared/*`, используемые сервисами
  (logger, errors, kafka-manager, grpc-client-manager, pg-boss-manager).
- [proto/](../proto/CONTEXT/purpose.md) — gRPC/protobuf-контракты (git-submodule), источник codegen'а.

Директории без собственного CONTEXT/: `make/` (фрагменты Makefile), `.github/`
(workflows CI/CD) — описаны ниже как инструменты корня.
