# Remy — сервис AI-бота

## Purpose

Сервис AI-ассистента (клиент OpenRouter/OpenAI-совместимого API). На момент анализа —
каркас: домен и use-cases продуктовой модели склонированы от gusteau, AI-клиент
написан, но не подключён к wiring.

## Abstraction level

Конкретный сервис внутри уровня [services/](../CONTEXT.md). Слои — по общему шаблону родителя.

## Parent

[../CONTEXT.md](../CONTEXT.md).

## Children

Нет дочерних CONTEXT.md.

## Responsibility

- gRPC-сервер `Health` на `:50051` (бизнес-RPC не зарегистрированы:
  `registerRemyService()` пуст, `RemyHandler` создаётся, но не привязан).
- `src/infrastructure/ai/` — `AiClient` интерфейс, `OpenRouterClient` (SDK `openai`),
  конфиг `OPENROUTER_API_KEY` / `AI_BASE_URL` / `AI_MODEL`.
- Prisma-модели `Product`, `NutritionCoefficient`, `FoodLog` (БД `remy`) — клон gusteau.

## Non-responsibility

- Пока не отвечает ни за один публичный RPC и ни один Kafka-топик
  (продюсеры/консьюмеры пусты, кроме health).
- Собственно распознавание речи → speech-to-text.

## Dependencies

`@shared/{logger,errors,kafka-manager,pg-boss-manager}`, `openai ^5`, `@prisma/client`,
`ws`, `jsonwebtoken`. `grpc-client-manager` не заявлен.

## Dependency direction

Как в родителе. Клиент `infrastructure/ai/` в импортах вне `ai/` не встречается —
граница «use-case → AI-порт» пока не проведена.

## Entry points

- gRPC `:50051` (только Health).
- `src/app.ts`.

## Structure

- `src/infrastructure/ai/` — `clients/openrouter.ts`, `configs/openrouter.ts`,
  `interfaces/{ai-client,ai-config}.interface.ts`; `service.ts` и `client.ts` здесь —
  битые (несуществующие импорты `./handlers/*`, синтаксическая ошибка в `service.ts`).
- `src/domain/{product,food-log,nutrition-coefficient}/`, `application/use-cases/*` —
  клон gusteau (см. [../gusteau/](../gusteau/CONTEXT.md)).
- `__tests__/functional/battle.test.ts` — нерабочая копия старого теста.
- Kafka/pg-boss-конфиги пустые; health-топик `healthcheck`.

## Architectural rules

- Существующие use-cases не импортируют `infrastructure/ai` — новый AI-use-case
  придётся подключать в `composition/build-application.ts` вручную.
- env: `OPENROUTER_API_KEY`, `AI_BASE_URL`, `AI_MODEL`; в k8s-конфигурации (удалённой)
  сервис числился с именем `remy` и консьюмером `remy.connecting-request`.

## Important files

- `src/infrastructure/ai/clients/openrouter.ts` — единственный AI-клиент в проекте.
- `src/composition/build-application.ts` — где подключается (и пока не подключён) AI.

## Navigation

- Модель продуктов → [../gusteau/](../gusteau/CONTEXT.md) (оригинал клона).
- Кто шлёт `bot.connecting-request` → [../streaming/](../streaming/CONTEXT.md).
