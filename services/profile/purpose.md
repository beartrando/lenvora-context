# Profile — gRPC-сервис профилей игрока: профиль, опыт, достижения

Владеет профилями пользователей (PostgreSQL через Prisma): ник, уровень, рейтинг, опыт, достижения.

gRPC: `Profile.GetOrCreate`, `Profile.Get`, `Profile.GetByUser` + `common.health.Health`, порт `GRPC_PORT` (50051).

Создаёт профиль автоматически по Kafka-событию `user.created` от [auth](../../services/auth/purpose.md) и публикует `profile.created`.

Не аутентифицирует: проверяет access-токен локально по `ACCESS_SECRET`; наружу доступен только через [orchestration](../../services/orchestration/purpose.md) и [gateway](../../services/gateway/purpose.md).
