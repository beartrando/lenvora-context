# Important files

- `src/composition/build-application.ts` — wiring: репозитории, use-case'ы, хендлеры, сервисы.
- `src/infrastructure/grpc/handlers/gusteau.handler.ts` — все gRPC-операции домена.
- `src/application/use-cases/**` — шесть сценариев (product, nutrition-coefficient, food-log) + health.
- `src/infrastructure/db/prisma/schema.prisma` — модели `Product`, `NutritionCoefficient`, `FoodLog`; рядом `migrations/20260930031017/migration.sql`.
- `src/infrastructure/db/repositories/*.ts` — Prisma-реализации портов домена; `src/infrastructure/db/mappers/*.ts` — маппинг в БД.
- `src/infrastructure/grpc/mappers/*.ts` — маппинг домен ↔ proto.
- `src/domain/**` — сущности и value object'ы с правилами валидации.
- `.env.dist`, `jest.config.js`, `tsconfig.json`, `Dockerfile`.
- `__tests__/functional/battle.test.ts` — ссылается на несуществующие модули, `pnpm test` на нём не проходит.
