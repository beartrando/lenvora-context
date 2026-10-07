# Entry points

- Точка входа кода: `services/orchestration/src/app.ts` → `composition/build-application.ts` → `Application.start()` (message → job → db → grpc).
- gRPC `0.0.0.0:${GRPC_PORT}` (50051): `Orchestration.Proxy(ProxyRequest) → ProxyResponse` и `common.health.Health` (Check/Status/Livez/Readyz).
- `ProxyRequest` = `{method, path, body: bytes, headers: [{key, value}], queryParams: [{key, value}]}`; `ProxyResponse` = `{statusCode, headers, body: bytes}` — тело здесь JSON.
- Маршруты (`infrastructure/proxy/routes`): `POST /auth/anonymousSignIn`, `POST /auth/refreshTokens`, `POST /auth/verify`, `GET /auth/getMe`, `GET /profile/getMe`, `GET /profile/get`.
- Env: `GRPC_PORT`, `DATABASE_URL`/`DATABASE_DIRECT_URL`, `ACCESS_SECRET`, `SERVICE_AUTH_URL`, `SERVICE_PROFILE_URL`, `SERVICE_GUSTEAU_URL`, `KAFKA_BROKERS`/`KAFKA_CLIENT_ID`, `PG_BOSS_*`, `REDIS_*`.
- Docker: `EXPOSE 50051`, CMD `pnpm --filter orchestration dev` / `node dist/app.js`, HEALTHCHECK `nc -z localhost 50051`; predeploy — `pnpm --filter orchestration migrate:deploy`.
- Compose: сервис `orchestration` (env_file `services/orchestration/.env`), `depends_on: kafka, pgbouncer`, publish-портов нет.
- Скрипты package.json: `dev`, `stage`, `build`, `start`, `test`, `proto:generate`, `prisma:generate`, `migrate*`, `seed`.
