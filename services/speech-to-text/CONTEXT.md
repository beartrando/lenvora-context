# Speech-to-Text — каркас сервиса распознавания речи

## Purpose

Заготовка сервиса преобразования речи в текст. На момент анализа — пустой каркас:
скопирован из gusteau, собственной логики (ни аудио, ни транскрипции, ни AI SDK) нет.

## Abstraction level

Конкретный сервис внутри уровня [services/](../CONTEXT.md). Слои — по общему шаблону родителя.

## Parent

[../CONTEXT.md](../CONTEXT.md).

## Children

Нет дочерних CONTEXT.md.

## Responsibility

- gRPC-сервер `Health` на `:50051` (бизнес-RPC не зарегистрированы:
  `registerSpeechToTextService()` пуст, `SpeechToTextHandler` — заглушка без методов).
- Структура сервиса: Prisma-модели `Product`/`NutritionCoefficient`/`FoodLog` (БД
  `speech-to-text`), домен и use-cases продуктовой модели — клоны gusteau.

## Non-responsibility

- Фактически не отвечает ни за что, кроме health: Kafka/pg-boss пустые, gRPC-клиентов нет,
  mappers отсутствуют, тестов нет (в `__tests__/` — нерабочая копия battle-теста).

## Dependencies

`@shared/{logger,errors,kafka-manager,pg-boss-manager}`, `@prisma/client`, `ws`,
`jsonwebtoken`. `grpc-client-manager` не заявлен.

## Dependency direction

Как в родителе (копия gusteau).

## Entry points

- gRPC `:50051` (только Health).
- `src/app.ts`.

## Structure

Идентична [../gusteau/](../gusteau/CONTEXT.md) за исключением:
`grpc/handlers/speech-to-text.handler.ts` (пустой), отсутствия `grpc/mappers/`,
незарегистрированного бизнес-сервиса.

## Architectural rules

- Зарегистрировать RPC можно только после появления контракта: отдельного
  `speech-to-text.proto` в [../../proto/](../../proto/CONTEXT.md) нет — контракт
  придётся добавлять туда.

## Git workflow

- Коммиты делаем в ветку `feature` (не в `dev` напрямую).
- Пушим в `origin`: `git push origin feature`.
- Пул-реквест через `gh` из `feature` в `dev`:
  `gh pr create --base dev --head feature --title "..." --body "..."`.

## Important files

- `src/infrastructure/grpc/service.ts` — пустая точка будущей регистрации.
- `src/composition/build-application.ts`.

## Navigation

- Реализация-прототип, из которого клонировался сервис → [../gusteau/](../gusteau/CONTEXT.md).
