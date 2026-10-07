# Dependency direction

- Зависит от: `@shared/logger`, `@grpc/grpc-js`; от `services/*` и других `@shared/*` не зависит (в том числе не зависит от [errors](../errors/purpose.md) — ошибки пробрасываются как `grpc.ServiceError`).
- Зависятся от неё (проверено grep'ом по `services/*`):
  - импорты в `src/`: `services/gateway` (`health.aggregator.ts`, `orchestration.client.ts`), `services/orchestration` (`auth.client.ts`, `engine.client.ts`, `profile.client.ts`), `services/streaming` (`profile.client.ts`);
  - заявлен в `package.json` и Dockerfile без импортов в `src/`: `services/auth`, `services/profile`;
  - в тестах: `services/gateway/src/infrastructure/grpc/clients/health.aggregator.spec.ts` мокает `GrpcClientManager`.
- Направление: `logger` → `grpc-client-manager` → `gateway`/`orchestration`/`streaming`.
