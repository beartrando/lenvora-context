# Dependencies

Внешние, по коду `services/*`:

- `@shared/logger`, `@shared/errors`, `@shared/kafka-manager`, `@shared/grpc-client-manager`, `@shared/pg-boss-manager` — workspace-пакеты из [shared](../shared/purpose.md) (перечислены в `pnpm-workspace.yaml`).
- Сгенерированные из [proto](../proto/purpose.md) контракты `src/contracts/proto/*`.
- `@grpc/grpc-js`, `@bufbuild/protobuf`, `ts-proto` — gRPC/protobuf.
- `@prisma/client` + `prisma` — PostgreSQL (у auth, profile, orchestration, streaming, gusteau, remy, speech-to-text; у gateway нет).
- `kafkajs` + `pg-boss` — события и очереди (gateway кода к ним не подключает).
- `jsonwebtoken`, `bcrypt` (auth), `express`/`morgan`/`cors` (gateway), `ws` (streaming), `ioredis` (orchestration), `openai` (remy).

Инфраструктура (docker-compose корня): postgres:17.6, pgbouncer, kafka:4.1.1, redis:7.2, node:22-slim, pnpm@11.28.4, turbo.

Сборка: `pnpm-workspace.yaml` (`services/*`, `shared/*`), `turbo.json` (задачи `build`, `dev`), `tsconfig.base.json` (ES2022, NodeNext, strict).
