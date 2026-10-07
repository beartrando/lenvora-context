# Entry points

- Запуск: `src/app.ts` → `buildApplication()` → `Application.start()` в порядке message → job (pg-boss) → gRPC → WebSocket; остановка в обратном порядке.
- gRPC-сервер: `0.0.0.0:GRPC_PORT` (50051), регистрируется только `Health`.
- WebSocket-сервер: `WEBSOCKET_PORT` (8080), маршрут `/event`.
- Скрипты `package.json`: `dev` (nodemon + ts-node), `stage`, `build` (tsc), `start`, `proto:generate`, `prisma:generate`, `migrate*`, `seed`, `test` (jest).
- Docker: `services/streaming/Dockerfile` (стадии deps/dev/build/prod), сервис `streaming` в docker-compose (healthcheck на 50051, монтируются `src` и `shared`).
- Env (`.env`, шаблон `.env.dist`): `GRPC_PORT`, `WEBSOCKET_PORT`, `DATABASE_URL`, `DATABASE_DIRECT_URL`, `SERVICE_AUTH_URL`, `SERVICE_PROFILE_URL`, `SERVICE_SPEECH_TO_TEXT_URL`, `ACCESS_SECRET`, `REFRESH_SECRET`, `KAFKA_BROKERS`, `KAFKA_CLIENT_ID`, `MESSAGE_CONSUMER_EVENT_FIRED`, `PG_BOSS_*`.
