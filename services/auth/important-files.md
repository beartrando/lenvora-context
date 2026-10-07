# Important files

- `src/composition/build-application.ts` — фактический wiring (use-cases, репозитории, handlers, конфиги).
- `src/application/use-cases/auth/anonymous-sign-in.ts` — главный сценарий: транзакция user → device → session + `user.created` в очередь; рядом `.spec.ts`.
- `src/infrastructure/grpc/handlers/auth.handler.ts` (+ `.spec.ts`) — разбор запросов и маппинг ошибок через `callback.error.ts`.
- `src/infrastructure/grpc/service.ts` — регистрация сервисов gRPC-сервера.
- `src/infrastructure/auth/service.ts` — генерация/верификация JWT; `src/infrastructure/auth/config.ts` — обязательные секреты и TTL.
- `src/infrastructure/db/prisma/schema.prisma` (+ `migrations/20260915160724`) — модели `users`, `sessions`, `devices`; `repositories/`, `mappers/`.
- `src/infrastructure/job/service.ts` — pg-boss и Kafka-bridge (без него событие `user.created` не уходит).
- `src/infrastructure/message/configs/*.ts` — брокер/clientId/groupId и топик `user.created`.
- `package.json`, `Dockerfile`, `.env.dist`, `jest.config.js` — команды, стадии сборки и состав переменных.
- `src/infrastructure/db/seed/seed.ts` — сиды сейчас закомментированы; скрипт `seed` package.json указывает на `src/infrastructure/db/seeds/seed.ts` — такого каталога нет.
