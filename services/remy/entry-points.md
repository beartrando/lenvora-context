# Entry points

- Запуск: `src/app.ts` → `buildApplication()` → `Application.start()`: message → job (pg-boss) → gRPC.
- gRPC-сервер: `0.0.0.0:GRPC_PORT` (50051), регистрируются `Remy` (`Process`) и `Health`.
- AI-вход (дочерний узел): `new AiService(aiConfig).process(text)`; конфиг — `infrastructure/ai/configs/openrouter.ts`.
- Скрипты `package.json`: `dev`, `stage`, `build`, `start`, `proto:generate`, `prisma:generate`, `migrate*`, `seed`, `test` (jest).
- Docker: `services/remy/Dockerfile`, сервис `remy` в docker-compose (healthcheck на 50051, `depends_on` только pgbouncer).
