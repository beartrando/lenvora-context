# Architectural rules

- Зависимости только внутрь: `domain` ← `application` ← `infrastructure`; новый код подключается в `composition/build-application.ts`, а не в `app.ts`.
- Контракты не пишутся руками: `src/contracts/proto` генерируется из [proto](../../proto/purpose.md) (`pnpm proto:generate`), каталог в git-игноре.
- `profile_id` фронт не присылает: значение инжектится сервисом в `audio_start` при переотправке в speech-to-text.
- Голосовая сессия одноразовая: реконнекта потока нет, разрыв WS закрывает поток (`close`), следующий `audio_start` открывает новый.
- Bidi-поток открывается отдельным grpc-js-клиентом, а не через `@shared/grpc-client-manager` (менеджер потоки не поддерживает).
- Внешние адреса берутся только из конфигов (`grpc.config.ts`, `websocket-server.config.ts`), дефолты — внутренние имена docker-compose.
