# Important files

- `src/composition/build-application.ts` — wiring: контроллеры, маршруты, клиенты, gRPC-сервер.
- `src/infrastructure/proxy/router.ts` — таблица маршрутов, 404 и обработка `TokenExpiredError` (401).
- `src/infrastructure/proxy/routes/{auth,profile}.routes.ts` — фактический список эндпоинтов API.
- `src/infrastructure/proxy/controllers/auth.controller.ts` — сценарий входа (auth → profile) и `getMe`/`verify`/`refreshTokens`.
- `src/infrastructure/proxy/controllers/profile.controller.ts` — `getMe`/`get` профиля.
- `src/infrastructure/grpc/handlers/orchestration.handler.ts` — gRPC `Proxy` → `Router.handle`.
- `src/infrastructure/grpc/clients/{auth,profile,engine}.client.ts` — адреса из `grpcConfig` (`engine` не используется).
- `src/infrastructure/proxy/{adapter/response.ts, utils/jwt-from-proxy-request.ts}` — формат ответа и извлечение токена.
- `src/infrastructure/auth/service.ts` — `verifyAccessToken` (`ACCESS_SECRET`).
- `src/infrastructure/db/prisma/schema.prisma` — только datasource (без моделей и migrations).
- `package.json`, `Dockerfile`, `.env.dist`; тестов (`*.spec.ts`) в сервисе нет.
