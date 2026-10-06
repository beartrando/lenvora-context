# Responsibility

- gRPC-сервер `Gusteau`: `createProduct`, `getProduct`, `getNutritionCoefficient`,
  `updateNutritionCoefficient`, `createFoodLog`, `getFoodLogs` + `Health`.
- Prisma-модели `Product`, `NutritionCoefficient`, `FoodLog` (БД `gusteau`).
- Proto-мапперы сущностей ↔ protobuf (единственный сервис с `grpc/mappers/`).
