# Structure

- `src/application/use-cases/profile/` — `get-or-create`, `get`, `get-by-user`; `health/`.
- `src/domain/profile/` — `profile.entity.ts`, `profile.repository.ts` (интерфейс),
  `value-objects/` (profile-id, nickname, level, rating, experience).
- `src/infrastructure/db/repositories/prisma-profile.repository.ts`,
  `mappers/profile.db.mapper.ts`, `prisma/schema.prisma`.
- `src/infrastructure/message/consumers/user-created.ts` — входящее событие.
- `src/infrastructure/grpc/handlers/profile.handler.ts`, `helpers/auth.helper.ts`.
- Тесты `*.spec.ts`.
