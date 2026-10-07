# Responsibility

- Приём HTTP-запросов клиента (express, CORS `origin: '*'`, morgan, `express.json`).
- Health-эндпоинты: `/gateway/health` (и `?full`), `/gateway/full-health`, `/gateway/status`, `/gateway/livez`, `/gateway/readyz` — агрегируют `common.health.Check` по всем `SERVICE_*_URL`.
- Проксирование всех прочих HTTP-запросов в [orchestration](../../services/orchestration/purpose.md) через gRPC `Orchestration.Proxy` и возврат ответа как есть (bytes → `application/x-protobuf`, JSON при `x-debug-json`).
- Отдельный gRPC-сервер health для вызовов изнутри инфраструктуры.
- Каталог сервисов — из переменных `SERVICE_*_URL` (health-агgregator проходит по всем).
