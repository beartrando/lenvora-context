# Structure

Слой Node.js/TypeScript-сервиса:

- `src/app.ts` — точка входа → `composition/build-application.ts` (единственный wiring).
- `src/application` — `Application`, use-case'ы `health/*`, порт `TokenServicePort`.
- `src/domain` — только VO (`UserId`, `ProfileId`, `SessionAccessToken`) и заготовки сущностей; схема БД без моделей.
- `src/infrastructure` — `grpc` (сервер, `SpeechToTextHandler` — каркас, `HealthHandler`), `message` (Kafka), `job` (pg-boss), `auth` (JWT-модуль, в wiring не подключён), `db` (Prisma), `app`-конфиг.
- `src/infrastructure/grpc/streams/engine.stream.container.ts` — единственный файл каталога `streams/`, импортирует несуществующие `../types/engine.stream` и `../../../contracts/proto/engine`: `tsc` падает с TS2307, поэтому `pnpm build` и стадия `build` в Dockerfile не проходят.
- `src/contracts/proto` — генерируемые типы (в git-игноре); локально `speech-to-text.ts` ещё не сгенерирован, генерация выполняется `pnpm proto:generate` и на стадии `deps` Dockerfile.
- Тестов нет: `jest.config.js` есть, `__tests__` отсутствует, `*.spec.ts` не найдены.
