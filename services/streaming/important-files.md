# Important files

- `src/composition/build-application.ts` — wiring всего сервиса, менять при добавлении любых компонентов.
- `src/application/use-cases/voice/voice-stream.ts` — голосовая сессия, основная логика; рядом `voice-stream.spec.ts` (единственный значимый тест).
- `src/infrastructure/grpc/clients/speech-to-text.client.ts` — bidi-клиент `Transcribe`, жизненный цикл потока.
- `src/infrastructure/websocket/connection.handler.ts` — токен, JWT, авторизация профиля, маршрутизация, heartbeat.
- `src/infrastructure/websocket/handlers/event.handler.ts` — диспетчеризация `audio_start`/`audio_chunk`/`audio_stop`.
- `src/infrastructure/websocket/server.service.ts` — WS-сервер, ping/pong, остановка.
- `src/infrastructure/grpc/service.ts` — gRPC-сервер и регистрация health.
- `src/infrastructure/websocket/registers/event-websockets.registry.ts`, `senders/event-websocket.sender.ts` — реестр сокетов и рассылка событий.
- `src/infrastructure/state-store/state-stores/state-store.ts` — in-memory хранилище (контексты сокетов, голосовые сессии).
- `.env.dist` — полный перечень настраиваемых переменных; `tsconfig.json`, `Dockerfile`; `jest.config.js` ссылается на отсутствующий `jest.setup.ts` — jest не стартует (ошибка валидации конфигурации).
