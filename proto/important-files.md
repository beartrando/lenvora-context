# Important files

- `<service>.proto` (auth, gusteau, orchestration, profile, remy, speech-to-text — gRPC-сервисы; streaming — только сообщения) — контракты сервисов.
- `common/error.proto`, `common/response.proto`, `common/empty.proto` — переиспользуемые типы ответов/ошибок.
- `common/health.proto`, `common/ping.proto`, `common/key_value.proto` — служебные типы.
- `.gitignore` — игнорирует `**/CONTEXT`.
