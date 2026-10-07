# Structure

Слой Node.js/TypeScript-сервиса (чистая архитектура/DDD):

- `src/app.ts` — точка входа → `composition/build-application.ts` (единственный wiring).
- `src/application` — `Application`, порт `TokenServicePort`, use-case'ы: `product/create|get`, `nutrition-coefficient/get|update`, `food-log/create|get`, `health/*`.
- `src/domain` — сущности `Product`, `NutritionCoefficient`, `FoodLog` + интерфейсы репозиториев + value object'ы; есть заготовки `profile`/`user`/`session` (VO и сущности без логики).
- `src/infrastructure` — `db` (Prisma-схема с тремя моделями, `migrations/20260930031017`, `repositories/`, `mappers/`, `seed/`), `grpc` (сервер, хендлер `GusteauHandler`, протокольные мапперы), `message` (Kafka), `job` (pg-boss), `auth` (JWT-конфиг и сервис), `app`-конфиг.
- `src/contracts/proto` — генерируемые типы из [proto](../../proto/purpose.md) (в git-игноре).
- `__tests__/functional/battle.test.ts` — единственный файл в `__tests__`, ссылается на несуществующие модули сервиса и не собирается.
