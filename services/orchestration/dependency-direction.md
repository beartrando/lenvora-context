# Dependency direction

- orchestration → [shared](../../shared/purpose.md) и → [proto](../../proto/purpose.md) (сгенерированные контракты).
- orchestration → [auth](../../services/auth/purpose.md) (gRPC `AuthClient`: anonymousSignIn, refreshTokens, logout, get) и → [profile](../../services/profile/purpose.md) (gRPC `ProfileClient`: getOrCreate, getByUser, get).
- Клиент [gusteau](../../services/gusteau/purpose.md) (`SERVICE_GUSTEAU_URL`, `infrastructure/grpc/clients/engine.client.ts`) создан, но в use-case'ах не вызывается.
- Клиенты создаются по адресам `SERVICE_*_URL`; обратных gRPC-вызовов к orchestration делают только [gateway](../../services/gateway/purpose.md) и health-агрегатор gateway.
- orchestration → PostgreSQL (своя БД `orchestration`, только health) и → Kafka/pg-boss (без топиков, health).
- Верификация JWT идёт локально по `ACCESS_SECRET`, поэтому зависимость от auth в рантайме — только на gRPC-вызовах.
