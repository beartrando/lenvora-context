# Structure

- `src/infrastructure/websocket/` — `server.service.ts`, `connection.handler.ts`,
  `handlers/event.handler.ts`, `registers/event-websockets.registry.ts`,
  `senders/{websocket,event-websocket}.sender.ts`, `configs/`, `types/`.
- `src/infrastructure/state-store/state-stores/state-store.ts` — generic Map-хранилище
  (файлы `service.ts` и `cached-id-state-store.ts` закомментированы).
- `src/infrastructure/grpc/clients/profile.client.ts`,
  `src/infrastructure/grpc/clients/speech-to-text.client.ts` (bidi, отдельный
  grpc-js клиент — `GrpcClientManager` потоки не поддерживает), `helpers/auth.helper.ts`.
- `src/application/use-cases/auth/authorize-profile.ts`,
  `use-cases/websocket/websocket-disconnect.ts`,
  `use-cases/voice/voice-stream.ts` — голосовая сессия: ленивый open стрима
  speech-to-text на `audio_start`, инжект `profile_id` в `audio_start`,
  доставка транскрипции/ошибок по WS тем же `EventResponse`-контрактом
  (парный тест `voice-stream.spec.ts`).
- Тесты — коллокейтед `src/**/*.spec.ts`; `job.config.ts` лежит в корне `job/`
  (не в `configs/`).
