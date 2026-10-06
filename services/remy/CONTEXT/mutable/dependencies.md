# Dependencies

`@shared/{logger,errors,kafka-manager,pg-boss-manager}`, `openai ^5`,
`@prisma/client` (клиент сгенерирован, моделей в schema.prisma нет).
`ws` и `jsonwebtoken` заявлены, но не импортируются (auth-слой удалён) —
зависимости не чистили.
`grpc-client-manager` не заявлен. Контракты — `src/contracts/proto/` (генерируются, в git не попадают).
