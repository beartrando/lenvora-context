# Dependencies

- `proto/` — внешний репозиторий `lenvora-proto` подключается напрямую CI-клиента
  (codegen в `lib/contracts/proto`).
- `gateway:9090` (HTTP), `streaming:8080` (WebSocket) — рантайм-зависимости.
- pubspec: flutter_riverpod, http, web_socket_channel, protobuf, firebase_*,
  shared_preferences, flutter_secure_storage.
