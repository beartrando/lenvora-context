# Important files

- `src/index.ts` — вся реализация: `GrpcClientManager.call/reconnect/isRecoverableError/execute`, типы `GrpcClientFactory`, `GrpcCall`.
- `package.json` — deps `@grpc/grpc-js`, `@shared/logger`; `exports["."]`, скрипты `build`/`dev`/`prepare`.
- `tsconfig.json`, `README.md` (usage + порядок релиза), `pnpm-workspace.yaml` (fallback на git+https).
- Примеры потребителей: `services/gateway/src/infrastructure/grpc/clients/orchestration.client.ts`, `services/orchestration/src/infrastructure/grpc/clients/auth.client.ts`, `services/streaming/src/infrastructure/grpc/clients/profile.client.ts`.
