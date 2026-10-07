# Entry points

- Точка входа кода: `services/auth/src/app.ts` → `composition/build-application.ts` → `Application.start()` (message → job → db → grpc).
- gRPC-сервер: `0.0.0.0:${GRPC_PORT}` (по умолчанию 50051), `bindAsync` в `infrastructure/grpc/service.ts`; регистрирует `AuthService` и `HealthService`.
- Методы (proto/auth.proto): `AnonymousSignIn(AnonymousSignInRequest{deviceId}) → SessionResponse`, `RefreshTokens(RefreshTokensRequest{token}) → SessionResponse`, `Logout(Empty) → SuccessResponse`, `Get(UserIdRequest) → UserResponse`; health: `Check/Status/Livez/Readyz`.
- Metadata: `authorization: Bearer <access token>` для `Logout`/`Get` (и вызывающие передают его же в `Get`).
- Docker: `EXPOSE 50051`, CMD `pnpm --filter auth dev` (dev) / `node dist/app.js` (prod), HEALTHCHECK `nc -z localhost 50051`; стадия predeploy — `pnpm --filter auth migrate:deploy`.
- Compose: сервис `auth` (env_file `services/auth/.env`), `depends_on: kafka, pgbouncer`, publish-портов нет, команда `pnpm dlx turbo run dev --parallel --filter auth`.
- Скрипты package.json: `dev`, `stage`, `build`, `start`, `test`, `proto:generate`, `prisma:generate`, `migrate`, `migrate:deploy`, `migrate:reset`, `seed`.
