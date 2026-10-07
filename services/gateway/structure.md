# Structure

```text
gateway/src/
├── app.ts                                 # buildApplication().start()
├── composition/build-application.ts       # wiring: healthAggregator, orchestrationClient, http + grpc
├── application/
│   ├── application.ts                     # lifecycle: grpc → http
│   └── use-cases/health/{health,full-health,status,livez,readyz}.ts (+ spec у health/full-health)
├── infrastructure/
│   ├── app/config.ts                      # APP_NAME/APP_VERSION/APP_ENV
│   ├── grpc/
│   │   ├── service.ts                     # только HealthService на GRPC_PORT
│   │   ├── config/{config,microservices.config}.ts  # порт + сбор SERVICE_*_URL
│   │   ├── clients/health.aggregator.ts   # HealthClient по каждому сервису
│   │   ├── clients/orchestration.client.ts# клиент gRPC Proxy
│   │   └── handlers/health.handler.ts (+ spec)
│   └── http/
│       ├── configs/http.server.config.ts  # HTTP_PORT (9090)
│       ├── server.service.ts              # express: /gateway/* + catch-all proxy
│       ├── routes/health.routes.ts        # GatewayRouter
│       ├── controllers/{gateway,proxy}-controller.ts, wrapper.ts
│       └── middlewares/request-logger.middleware.ts
├── infrastructure/orchestration/service.ts # сбор ProxyRequest и разбор ответа
└── contracts/proto/                       # сгенерированный код (в .gitignore)
```

Prisma/Kafka/pg-boss в gateway отсутствуют; `domain/` нет — сервис тонкий.
