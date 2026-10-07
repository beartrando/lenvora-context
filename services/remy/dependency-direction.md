# Dependency direction

- Внутрь: `src/app.ts` → `composition/build-application.ts` → (`application`, `infrastructure`, `domain`).
- `application` зависит от `domain` и контрактов (`contracts/proto/remy`); не импортирует gRPC/Kafka/SDK.
- `infrastructure/ai` зависит только от собственных `interfaces/`; наружу уходит по HTTPS к `AI_BASE_URL`.
- Направление вызова: вызывающая сторона → remy:50051 (`Remy.Process`); remy ни к какому сервису не обращается.
- Обратная сторона — ответ в том же потоке вызова; Kafka и pg-boss используются только для инфраструктурных очередей/health.
