# Structure

- `src/infrastructure/grpc/handlers/gusteau.handler.ts`,
  `mappers/{product,food-log,nutrition-coefficients}.proto.mapper.ts`.
- `src/domain/{product,food-log,nutrition-coefficient}/` — сущности + интерфейсы репозиториев.
- `src/application/use-cases/{product,food-log,nutrition-coefficient}/` — 6 use-cases + health.
- `src/infrastructure/db/repositories/prisma-*.repository.ts`, `prisma/schema.prisma`.
- Kafka/pg-boss: конфиги пустые (нет топиков), используется только health-проверка.
- Тесты: `__tests__/functional/battle.test.ts` — нерабочая копия (импортирует
  несуществующие `contracts/proto/battle`, `domain/battle`).
