# Architectural rules

- Только gRPC: HTTP-поверхности нет, наружу auth не ходит; клиенты — [orchestration](../../services/orchestration/purpose.md) (и любой вызывающий по `SERVICE_AUTH_URL`).
- Регистрация/сессии — в одной `DbService.transaction`; событие `user.created` ставится в очередь через `JobService.enqueue` (pg-boss → Kafka bridge), не напрямую в Kafka.
- Токены: access/refresh подписываются разными секретами, `subject` = userId; верификация возвращает `UserId`, `TokenExpiredError` мапится в `@shared/errors`.
- `Logout` и `RefreshTokens` верифицируют токен (`verifyAccessToken`/`verifyRefreshToken`); `Get` требует заголовок `authorization`, но внутри auth токен не проверяется — проверка на вызывающей стороне.
- Обязательные env бросают ошибку при старте (`required("ACCESS_SECRET")`, `required("REFRESH_SECRET")`).
- Prisma-модели изолированы в `infrastructure/db`: домен работает через интерфейсы репозиториев, мапперы — отдельно (db-mapper и proto-mapper).
- Новые методы добавляются в `proto/auth.proto`, затем `pnpm proto:generate` и регистрация в `infrastructure/grpc/service.ts` (`registerAuthService`).
