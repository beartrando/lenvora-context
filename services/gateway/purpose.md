# Gateway — единая HTTP-точка входа: health-агрегация и проксирование в orchestration

Express-сервер на `HTTP_PORT` (9090), единственная публикуемая наружу порт-поверхность API.

Пропускает health-маршруты (`/gateway/*`) и весь остальной трафик упаковывает в gRPC `Orchestration.Proxy` (метод, путь, тело, заголовки, query).

Плюс собственный gRPC-сервер на `GRPC_PORT` (50051) с `common.health.Health` для общей health-агрегации (`SERVICE_*_URL`).

Без БД, Kafka и prisma — только маршрутизация и агрегация.
