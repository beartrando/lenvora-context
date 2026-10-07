# Dependency direction

- gateway → [shared](../../shared/purpose.md) и → [proto](../../proto/purpose.md) (сгенерированные контракты).
- gateway → [orchestration](../../services/orchestration/purpose.md) — единственная бизнес-зависимость (gRPC `Proxy`); обратно orchestration gateway не вызывает.
- gateway → остальные сервисы только health-вызовами `common.health.Check` по адресам `SERVICE_*_URL` (auth, profile, remy, gusteau, streaming, speech-to-text, orchestration).
- Прямых зависимостей от БД, Kafka и pg-boss нет.
- [front](../../services/front/purpose.md) и внешние клиенты зависят от gateway как от единственной HTTP-точки входа.
- Обратная зависимость «сервисы → gateway» в коде отсутствует.
