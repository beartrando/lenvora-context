# Entry points

- Запуск: `src/app.ts` → `buildApplication()` → `Application.start()`: message → job (pg-boss) → gRPC.
- gRPC-сервер: `0.0.0.0:GRPC_PORT` (50051), регистрируются `Gusteau` и `Health`.
- Скрипты `package.json`: `dev`, `stage`, `build`, `start`, `proto:generate`, `prisma:generate`, `migrate`, `migrate:deploy`, `migrate:reset`, `seed`, `test` (jest).
- Docker: `services/gusteau/Dockerfile`, сервис `gusteau` в docker-compose (healthcheck на 50051).
- Env (`.env`, шаблон `.env.dist`): `GRPC_PORT`, `DATABASE_URL`, `DATABASE_DIRECT_URL`, `KAFKA_BROKERS`, `KAFKA_CLIENT_ID`, `MESSAGE_PRODUCER_EVENT_FIRED`, `PG_BOSS_*`, `ACCESS_SECRET`.
