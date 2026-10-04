# Streaming — WebSocket-сервис

## Purpose

Сервер WebSocket для событий в реальном времени: авторизация соединения по токену,
проверка профиля через gRPC, реестр открытых сокетов и отправка событий клиенту.

## Abstraction level

Конкретный сервис внутри уровня [services/](../CONTEXT.md). Слои — по общему шаблону родителя.

## Parent

[../CONTEXT.md](../CONTEXT.md).

## Children

Нет дочерних CONTEXT.md.

## Responsibility

- WebSocket-сервер (`WEBSOCKET_PORT ?? 8080`) с heartbeat-циклом (5s ping).
- `ConnectionHandler` — аутентификация (`?token=` или Bearer), авторизация профиля
  (gRPC `profile.GetByUser`), кодирование (protobuf по умолчанию / JSON при `x-debug-json`),
  диспатч по путям (`/event`), обработка закрытия.
- Реестр `EventWebsocketsRegistry` на двух in-memory `StateStore`
  (socket → контекст, profileId → socket) и `EventWebsocketSender` (отправка `EventObject`).
- gRPC-сервер `Health` на `:50051`.
- Kafka: produce `bot.connecting-request` (единственный продюсер-топик, через pg-boss-мост).

## Non-responsibility

- HTTP-трафика (→ gateway), бизнес-логики данных (→ gusteau/profile), БД-моделей
  (Prisma-схема без моделей, `DbService` не подключён).

## Dependencies

`@shared/{logger,errors,grpc-client-manager,kafka-manager,pg-boss-manager}`, `ws`,
`@prisma/client`, `ioredis` (заявлен, не используется), `jsonwebtoken`.

## Dependency direction

Как в родителе. Специфика: `domain/` — только value-objects; use-case
`authorize-profile` зависит от gRPC-клиента и `AuthHelper`, use-case
`websocket-disconnect` — от реестра сокетов.

## Entry points

- WebSocket `:8080` (в compose/k8s — публикуемый порт).
- gRPC `:50051` (только Health).
- `src/app.ts`.

## Structure

- `src/infrastructure/websocket/` — `server.service.ts`, `connection.handler.ts`,
  `handlers/event.handler.ts`, `registers/event-websockets.registry.ts`,
  `senders/{websocket,event-websocket}.sender.ts`, `configs/`, `types/`.
- `src/infrastructure/state-store/state-stores/state-store.ts` — generic Map-хранилище
  (файлы `service.ts` и `cached-id-state-store.ts` закомментированы).
- `src/infrastructure/grpc/clients/profile.client.ts`, `helpers/auth.helper.ts`.
- `src/application/use-cases/auth/authorize-profile.ts`,
  `use-cases/websocket/websocket-disconnect.ts`.
- Тестов нет; `job.config.ts` лежит в корне `job/` (не в `configs/`).

## Architectural rules

- `EventHandler.handle()` сейчас пуст — отправка событий инициируется извне через
  `EventWebsocketSender`.
- Закомментированные заглушки: `grpc/handlers/{battle,profile}.service.ts`,
  `message/consumers/battle-started.ts` — остатки прошлой архитектуры, не рабочий код.
- env: `MESSAGE_PRODUCER_TOPIC_BOT_CONNECTING_REQUEST`, `MESSAGE_CONSUMER_EVENT_FIRED`
  (объявлен в `.env.dist`, но консьюмер не зарегистрирован).

## Git workflow

- Коммиты делаем в ветку `feature` (не в `dev` напрямую).
- Пушим в `origin`: `git push origin feature`.
- Пул-реквест через `gh` из `feature` в `dev`:
  `gh pr create --base dev --head feature --title "..." --body "..."`.

## Important files

- `src/infrastructure/websocket/connection.handler.ts` — весь lifecycle соединения.
- `src/composition/build-application.ts` — wiring реестров, сокервера, use-cases.

## Navigation

- Поверхность событийного контракта → [../../proto/](../../proto/CONTEXT.md)
  (`streaming.proto`, `gusteau.proto` `EventObject`).
- Кто читает `bot.connecting-request` → внешний потребитель (в репозитории не представлен).
