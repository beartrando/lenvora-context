# Dependencies

Внутри монорепозитория:

- [gateway](../gateway/purpose.md) — HTTP `GATEWAY_HOST:GATEWAY_PORT` (по умолчанию 9090), эндпоинты `/auth/anonymousSignIn`, `/auth/refreshTokens`, `/auth/verify`, `/profile/getMe`, `/profile/get`.
- [streaming](../streaming/purpose.md) — WebSocket `STREAMING_HOST:STREAMING_PORT` (8080), URI `ws://host:port/<path>?token=...`; инфраструктура есть, экраны её не открывают.
- [proto](../../proto/purpose.md) — исходники для `protoc --dart_out` (генерация на уровне корня и в CI).

Пакеты: `flutter`, `flutter_riverpod`, `web_socket_channel`, `protobuf`, `http`, `flutter_dotenv`, `shared_preferences`, `flutter_secure_storage`, `device_info_plus`, `jwt_decode`, `uuid`, `intl`, `logger`, `in_app_update`, `firebase_core`, `firebase_crashlytics`, `firebase_analytics`; dev: `flutter_test`, `protoc_plugin`, `flutter_lints`, `intl_utils`, `flutter_launcher_icons`, `flutter_native_splash`.

Платформы: android (`org.beartrando.lenvora`), ios, linux, macos, web, windows. CI (`.github/workflows/deploy.yml`) собирает App Bundle и публикует в Google Play.
