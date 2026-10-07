# Important files

- `src/composition/build-application.ts` — wiring и регистрация Kafka-consumer'а `user.created`.
- `src/application/use-cases/profile/get-or-create.ts` (+ `.spec.ts`) — создание профиля и публикация `profile.created`.
- `src/application/use-cases/profile/get.ts`, `get-by-user.ts` (+ `.spec.ts`) — чтение с NotFoundError.
- `src/infrastructure/grpc/handlers/profile.handler.ts` (+ `.spec.ts`) — gRPC-обработчики, маппинг ошибок `callback.error.ts`.
- `src/infrastructure/grpc/service.ts` — регистрация `ProfileService`/`HealthService`.
- `src/infrastructure/message/consumers/user-created.ts` — обработчик события из [auth](../../services/auth/purpose.md).
- `src/infrastructure/db/prisma/schema.prisma` (+ `migrations/20260915160728`) — `profiles`, `profile_achievements`, `achievements`, `experience_levels`.
- `src/infrastructure/db/repositories/prisma-profile.repository.ts`, `mappers/profile.db.mapper.ts`, `grpc/mappers/profile.proto.mapper.ts`.
- `src/infrastructure/auth/service.ts` — verifyAccessToken (`ACCESS_SECRET`).
- `src/infrastructure/db/seed/profile.seed.ts` — сид 100 профилей; скрипт `seed` package.json указывает на несуществующий путь `db/seeds/`.
- `package.json`, `Dockerfile`, `.env.dist`.
