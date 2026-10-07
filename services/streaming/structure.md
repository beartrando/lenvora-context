# Structure

Node.js/TypeScript-сервис без HTTP-фреймворка, слоистая структура:

- `src/app.ts` — точка входа, только вызывает `composition/build-application.ts`.
- `src/composition/build-application.ts` — единственный wiring-файл: сервисы, use-case'ы, gRPC- и WS-хендлеры, маршруты.
- `src/application` — `Application` (start/stop), use-case'ы: `auth/authorize-profile`, `voice/voice-stream`, `websocket/websocket-disconnect`, `health/*`, порт `TokenServicePort`.
- `src/domain` — только value object'ы (`UserId`, `ProfileId`, `SessionAccessToken`, общий `Id`), сущностей и репозиториев нет.
- `src/infrastructure` — `auth` (JWT), `grpc` (сервер health, клиенты profile и speech-to-text), `websocket` (сервер, connection/event-хендлеры, registry, senders), `state-store` (in-memory Map), `message` (Kafka), `job` (pg-boss), `db` (Prisma, моделей нет), `app`-конфиг.
- `src/contracts/proto` — типы, сгенерированные из [proto](../../proto/purpose.md); каталог в git-игноре, обновляется `pnpm proto:generate`.
- `src/infrastructure/grpc/handlers/battle.service.ts`, `profile.service.ts`, `grpc-stream/`, `message/consumers/battle-started.ts` — файлы, целиком закомментированные или не подключённые к wiring.
- `__tests__/` — пусто (тесты лежат как `*.spec.ts` рядом с кодом).
