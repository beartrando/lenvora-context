# Responsibility

- UI-экраны и роутинг (`lib/features/*`, `lib/router.dart`, `lib/app.dart`).
- Домен клиента (`lib/domain/{profile,setting}`).
- Инфраструктура клиента: HTTP к gateway (`GATEWAY_PORT`, по умолчанию 9090),
  WebSocket к streaming (`ws://host:port/<path>?token=...`), хранилища, auth,
  локализация, protobuf-контракты в Dart.
- Сборка и релиз: собственный `Makefile`, `android/gradlew`, GitHub Actions
  (`.github/workflows/deploy.yml` → Google Play internal/production).
