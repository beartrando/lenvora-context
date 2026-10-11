# Structure

- `src/application/use-cases/auth/` — `anonymous-sign-in`, `refresh-tokens`, `logout`, `get-user`;
  `use-cases/health/` — `health`, `readyz`.
- `src/infrastructure/grpc/` — `service.ts` (регистрация сервисов), `handlers/auth.handler.ts`,
  `helpers/auth.helper.ts` (Bearer из metadata), `mappers/user.proto.mapper.ts`.
- `src/infrastructure/db/` — `service.ts`, `repositories/prisma-{user,session,device}.repository.ts`,
  `prisma/schema.prisma`, `seed/`.
- `src/infrastructure/auth/`, `message/`, `job/` — JWT, Kafka, pg-boss.
- Тесты: `*.spec.ts` рядом с кодом (use-cases + `auth.handler.spec.ts`).
