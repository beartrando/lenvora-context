# Gusteau — сервис кулинарной домена

## Purpose

Справочный и учётный сервис питания: продукты, нутриентные коэффициенты, журнал еды.
Эталонная реализация «продуктовой» доменной модели (remy и speech-to-text клонировались
от него).

## Abstraction level

Конкретный сервис внутри уровня [services/](../CONTEXT.md). Слои — по общему шаблону родителя.

## Parent

[../CONTEXT.md](../CONTEXT.md).

## Children

Нет дочерних CONTEXT.md.

## Responsibility

- gRPC-сервер `Gusteau`: `createProduct`, `getProduct`, `getNutritionCoefficient`,
  `updateNutritionCoefficient`, `createFoodLog`, `getFoodLogs` + `Health`.
- Prisma-модели `Product`, `NutritionCoefficient`, `FoodLog` (БД `gusteau`).
- Proto-мапперы сущностей ↔ protobuf (единственный сервис с `grpc/mappers/`).

## Non-responsibility

- Генерацией событий владеет контракт `gusteau.proto` (`EventObject`), но отправку
  событий выполняет streaming; AI-логика → remy; профили → profile.

## Dependencies

`@shared/{logger,errors,kafka-manager,pg-boss-manager}`, `@prisma/client`, `ws`,
`jsonwebtoken`. `grpc-client-manager` не заявлен.

## Dependency direction

Как в родителе. gRPC-хендлеры → proto-мапперы → use-cases → домен/Prisma-репозитории.

## Entry points

- gRPC `:50051` — `Gusteau` и `Health`.
- `src/app.ts`.

## Structure

- `src/infrastructure/grpc/handlers/gusteau.handler.ts`,
  `mappers/{product,food-log,nutrition-coefficients}.proto.mapper.ts`.
- `src/domain/{product,food-log,nutrition-coefficient}/` — сущности + интерфейсы репозиториев.
- `src/application/use-cases/{product,food-log,nutrition-coefficient}/` — 6 use-cases + health.
- `src/infrastructure/db/repositories/prisma-*.repository.ts`, `prisma/schema.prisma`.
- Kafka/pg-boss: конфиги пустые (нет топиков), используется только health-проверка.
- Тесты: `__tests__/functional/battle.test.ts` — нерабочая копия (импортирует
  несуществующие `contracts/proto/battle`, `domain/battle`).

## Architectural rules

- Авторизация RPC в этом сервисе не настроена (`AuthService` не создаётся в composition).
- `createProduct` типизирует зависимость конкретным `PrismaProductRepository`
  (не доменным интерфейсом) — факт, зафиксирован как есть.

## Git workflow

- Коммиты делаем в ветку `feature` (не в `dev` напрямую).
- Пушим в `origin`: `git push origin feature`.
- Пул-реквест через `gh` из `feature` в `dev`:
  `gh pr create --base dev --head feature --title "..." --body "..."`.

## Important files

- `src/infrastructure/grpc/handlers/gusteau.handler.ts` — вся RPC-поверхность.
- `src/infrastructure/db/prisma/schema.prisma`.

## Navigation

- Контракт RPC → [../../proto/](../../proto/CONTEXT.md) (`gusteau.proto`).
- Потребители событий `EventObject` → [../streaming/](../streaming/CONTEXT.md).
