# Entry points

- Точка входа кода: `services/gateway/src/app.ts` → `composition/build-application.ts` → `Application.start()` (grpc → http).
- HTTP `0.0.0.0:${HTTP_PORT}` (по умолчанию 9090), публикуется compose'ом как `9090:9090`:
  - `GET /gateway/health` (query `?full` → агрегированные отчёты), `GET /gateway/full-health`, `GET /gateway/status`, `GET /gateway/livez`, `GET /gateway/readyz`;
  - любой другой метод/путь → `ProxyController` → gRPC `Orchestration.Proxy`.
- gRPC `0.0.0.0:${GRPC_PORT}` (50051): только `common.health.Health` (Check/Status/Livez/Readyz); `Check` агрегирует отчёты всех `SERVICE_*_URL`.
- Env: `HTTP_PORT`, `GRPC_PORT`, `APP_NAME/APP_VERSION/APP_ENV`, `SERVICE_ORCHESTRATION_URL` (прокси), `SERVICE_AUTH_URL`, `SERVICE_PROFILE_URL`, `SERVICE_REMY_URL`, `SERVICE_GUSTEAU_URL`, `SERVICE_STREAMING_URL`, `SERVICE_SPEECH_TO_TEXT_URL` (health).
- Docker: `EXPOSE 50051 9090`, CMD `pnpm --filter gateway dev` / `node dist/app.js`; HEALTHCHECK `curl http://localhost:9090/livez` — фактический маршрут закреплён за префиксом `/gateway`, т.е. `/gateway/livez`; compose healthcheck использует `wget http://127.0.0.1:50051`.
- Скрипты package.json: `dev`, `stage`, `build`, `start`, `test`, `proto:generate` (prisma-скриптов нет).
