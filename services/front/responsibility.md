# Responsibility

- Экраны и маршруты: `/` (home), `/settings`, `/profile`; тема и локализация через `flutter_localizations`.
- Анонимная авторизация: `anonymousSignIn` по `deviceId`, `refreshTokens`, `verify`; токены в secure storage, повторное создание access token при старте.
- HTTP-доступ: `HttpConfig`/`HttpClient`/`AuthorizedHttpClient` на gateway (`/auth/*`, `/profile/*`), конвертация proto-ответов.
- WebSocket-инфраструктура к streaming (`StreamingConfig`, `WebsocketConnection`, heartbeat 10 с) — к экранам не подключена.
- Настройки (SharedPreferences + `SettingDomain`), идентификация устройства, проверка обновлений (`in_app_update`, Android).
- Firebase: инициализация best-effort, Crashlytics-репортинг ошибок (Android/iOS/macOS), аналитика; на Linux инициализация Firebase пропускается, репортёр на web/windows не подключается.
- Генерация env-файлов и protobuf-контрактов (`env/generate.dart`, `make proto-generate`).
