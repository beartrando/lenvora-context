# Structure

- `src/infrastructure/http/` — `server.service.ts`, `routes/health.routes.ts`,
  `controllers/{gateway,proxy}-controller.ts`, `configs/http.server.config.ts`.
- `src/infrastructure/grpc/clients/` — `health.aggregator.ts`, `orchestration.client.ts`.
- `src/infrastructure/orchestration/service.ts` — Request → `ProxyRequest`.
- `src/infrastructure/grpc/config/microservices.config.ts` — карта `SERVICE_*_URL`.
- `src/application/use-cases/` — health/status/livez/readyz; тесты `*.spec.ts`.
