# Dependencies

Код (`services/gateway/package.json`):

- `express` (5.x), `morgan`, `cors` — HTTP-сервер; `cookie-parser` объявлен, но в `server.service.ts` подключение закомментировано.
- `@shared/grpc-client-manager` — менеджер переиспользования gRPC-каналов (health-aggregator, orchestration-client); `@shared/logger`, `@shared/errors` (`DomainError` в `callback.error.ts`) — [shared](../../shared/purpose.md).
- `@grpc/grpc-js`, `@bufbuild/protobuf`, `ts-proto` (dev) — gRPC и генерация контрактов из [proto](../../proto/purpose.md): `orchestration.ts`, `common/health.ts` и др.
- `jsonwebtoken` и `pg-boss` объявлены, но в коде gateway не используются (JWT проверяется в [orchestration](../../services/orchestration/purpose.md)).
- jest/ts-jest/supertest (dev) — тесты; nodemon/ts-node/typescript — dev-запуск.

Инфраструктура: postgres/kafka/redis не нужны; внешние адреса — только из `SERVICE_*_URL`.
