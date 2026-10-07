# Important files

- `lib/main.dart` — bootstrap и обработка ошибок (zones, Crashlytics).
- `lib/app.dart`, `lib/router.dart`, `lib/theme.dart` — приложение, маршруты, тема.
- `pubspec.yaml` — зависимости, `flutter_intl`, иконки/сплэш, assets (`.env`).
- `env/generate.dart` + `.env`, `.env.dev`, `.env.prod`, `.env.uat` — окружения; `Makefile` (только `env-generate`).
- `lib/infrastructure/http/config/http_config.dart` — адрес gateway из env.
- `lib/infrastructure/auth/auth_service.dart`, `lib/infrastructure/auth/api/auth_client.dart` — анонимная авторизация и токены.
- `lib/infrastructure/websocket/websocket_connection.dart`, `config/streaming_config.dart` — WS-слой к streaming.
- `lib/infrastructure/proto/converters/**` — конвертация proto-ответов в доменные сущности.
- `lib/contracts/proto/**` — генерируемые контракты (не править вручную).
- `analysis_options.yaml` — исключения анализатора; `.github/workflows/deploy.yml` — сборка и публикация.
