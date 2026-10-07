# Entry points

- Запуск: `src/app.ts` → `buildApplication()` → `Application.start()`: message → job (pg-boss) → gRPC.
- gRPC-сервер: `0.0.0.0:GRPC_PORT` (50051); `registerSpeechToTextService()` пуст, фактически зарегистрирован только `Health`.
- Скрипты `package.json`: `dev`, `stage`, `build` (tsc), `start`, `proto:generate`, `prisma:generate`, `migrate*`, `test` (jest).
- Docker: `services/speech-to-text/Dockerfile` — стадии deps (генерация proto и Prisma-клиента), dev (CMD `pnpm dev`, EXPOSE 50051), build (tsc + `pnpm deploy`), prod; сервис `speech-to-text` в docker-compose.
- Env (`.env`, шаблон `.env.dist`): `GRPC_PORT`, `DATABASE_URL`, `DATABASE_DIRECT_URL`, `KAFKA_BROKERS`, `KAFKA_CLIENT_ID`, `MESSAGE_PRODUCER_EVENT_FIRED`, `PG_BOSS_*`, `ACCESS_SECRET`.
