# Architectural rules

- Правки клиента не требуют пересборки backend-пакетов; правки `proto/` требуют
  повторного Dart-codegen (см. корневой `make proto-generate`, `FLUTTER_SERVICES := front`).
- front не входит ни в docker-compose, ни в deploy-процесс backend-сервисов.
