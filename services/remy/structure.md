# Structure

Слой Node.js/TypeScript-сервиса:

- `src/app.ts` — точка входа → `composition/build-application.ts` (единственный wiring).
- `src/application` — `Application`, use-case'ы `process/process.ts` и `health/*`, порт `TokenServicePort`.
- `src/domain` — только VO (`UserId`, `ProfileId`, `SessionAccessToken`) и заготовки сущностей `user`/`profile`; схема БД без моделей.
- `src/infrastructure` — `grpc` (сервер, `RemyHandler`), `ai/` (дочерний узел: `service.ts`, `clients/`, `configs/`, `interfaces/`), `message` (Kafka), `job` (pg-boss), `auth` (JWT), `db` (Prisma), `app`-конфиг.
- `src/contracts/proto` — генерируемые типы из [proto](../../proto/purpose.md) (в git-игноре).
- Тесты: `src/application/use-cases/process/process.spec.ts`, `src/infrastructure/grpc/handlers/remy.handler.spec.ts`, `src/infrastructure/ai/service.spec.ts`, `jest.setup.ts` (мокает `@shared/logger`).
