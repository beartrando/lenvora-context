# Entry points

- Приложение: `lib/main.dart` → `lib/app.dart` (`MaterialApp`) → `lib/router.dart` (`/`, `/settings`, `/profile`).
- Платформенные входы: `android/`, `ios/`, `linux/`, `macos/`, `web/`, `windows/` (стандартные для Flutter).
- Окружения: `.env` (создаётся из `.env.dev`/`.env.prod`/`.env.uat`), переменные `ENV`, `GATEWAY_HOST`, `GATEWAY_PORT`, `STREAMING_HOST`, `STREAMING_PORT`; `.env` включён в assets.
- Скрипты: `make -C services/front env-generate` (генерация env), `make proto-generate` на уровне корня (генерация Dart-контрактов), `flutter run`/`flutter build`, `flutter test`.
- CI: `.github/workflows/deploy.yml` — версионирование по тегам, генерация env и proto, сборка App Bundle, публикация в Google Play (internal/production).
- Docker-сервиса в docker-compose у front нет — приложение запускается локально на Flutter.
