# Structure

- `src/infrastructure/proxy/` — `router.ts`, `controllers/{auth,profile}.controller.ts`,
  `routes/{auth,profile}.routes.ts`, `middlewares/`, `adapter/response.ts`,
  `utils/{parse-body,parse-query-params,jwt-from-proxy-request}.ts`.
- `src/infrastructure/grpc/clients/` — `auth.client.ts`, `profile.client.ts`,
  `engine.client.ts` (клиент gusteau объявлен, но не используется).
- `src/application/use-cases/{auth,profile,health}/`.
- `src/infrastructure/cache/` — Redis `CacheService` **не подключён** (не создаётся в
  `build-application.ts`, конфиг пуст).
- `src/infrastructure/db/prisma/schema.prisma` — без моделей; тестов нет.
