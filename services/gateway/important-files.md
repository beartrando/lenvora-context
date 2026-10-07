# Important files

- `src/composition/build-application.ts` — wiring: aggregator, orchestration-client, роуты, http/grpc сервисы.
- `src/infrastructure/http/server.service.ts` — express-конфигурация, префикс `/gateway` и catch-all прокси.
- `src/infrastructure/http/routes/health.routes.ts` — список health-маршрутов.
- `src/infrastructure/http/controllers/proxy-controller.ts` — превращение HTTP-запроса в `ProxyRequest` и ответ клиенту (`x-debug-json`).
- `src/infrastructure/orchestration/service.ts` — сборка `ProxyRequest` (method/path/body/headers/query).
- `src/infrastructure/grpc/clients/orchestration.client.ts` — gRPC-клиент orchestration; `clients/health.aggregator.ts` — health по всем `SERVICE_*_URL`.
- `src/infrastructure/grpc/config/microservices.config.ts` — преобразование `SERVICE_*_URL` в каталог сервисов.
- `src/application/use-cases/health/{health,full-health}.ts` (+ `.spec.ts`) — агрегация и статус `OK/FAIL`.
- `src/healthcheck.js` — старый скрипт, импортирует несуществующий `contracts/proto/gateway`, в работе не используется.
- `package.json`, `Dockerfile`, `.env.dist`, `jest.config.js`.
