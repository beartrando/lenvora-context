# Architectural rules

- Только gRPC: HTTP нет; сервер регистрирует `ProfileService` и `HealthService` на `GRPC_PORT` (50051).
- `GetOrCreate` верифицирует access-токен из metadata (`AuthHelper.getAccessToken` → `verifyAccessToken`); `Get` и `GetByUser` токен не проверяют — проверка на вызывающей стороне ([orchestration](../../services/orchestration/purpose.md)).
- Идемпотентность: `GetOrCreate` сначала `findByUserId`, потом `save`; событие `profile.created` ставится в очередь через `JobService.enqueue` после успешного сохранения.
- Kafka-consumer регистрируется в composition (`messageService.addConsumer(userCreatedTopic, …)`), groupId формируется как `${groupId}-${topic}`; ошибки обработки события логируются и не роняют процесс.
- Домен не знает о Prisma: `ProfileRepository` — интерфейс в domain, реализация и мапперы в `infrastructure/db`.
- Новые поля/методы: правка `proto/profile.proto` + `pnpm proto:generate` + мапперы `profile.proto.mapper.ts` / `profile.db.mapper.ts`.
- Обязательный env — только `ACCESS_SECRET` (`required()` бросает при отсутствии).
