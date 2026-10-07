# Auth — gRPC-сервис аутентификации: анонимный вход, JWT-сессии, пользователи и устройства

Владеет пользователями, устройствами и сессиями (PostgreSQL через Prisma) и выдаёт/верифицирует JWT.

gRPC: `Auth.AnonymousSignIn`, `Auth.RefreshTokens`, `Auth.Logout`, `Auth.Get` + `common.health.Health`, порт `GRPC_PORT` (50051).

Производит Kafka-событие `user.created` (через pg-boss bridge) — потребляет его [profile](../../services/profile/purpose.md).

Не знает о профилях и бизнес-логике выше аутентификации; HTTP-доступ к нему идёт только через [gateway](../../services/gateway/purpose.md) → [orchestration](../../services/orchestration/purpose.md).
