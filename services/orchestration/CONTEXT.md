# Orchestration — внутренний маршрутизатор

## Purpose

Внутренний API-роутер: принимает `ProxyRequest` от gateway по gRPC, маршрутизирует по
путям к use-cases, выполняет транспортную авторизацию (JWT) и возвращает `ProxyResponse`.

## Abstraction level

Конкретный сервис внутри уровня [services/](../CONTEXT.md). Слои — по общему шаблону родителя.

## Parent

[../CONTEXT.md](../CONTEXT.md).

## Children

Нет дочерних CONTEXT.md.

## Responsibility

- gRPC-сервер `Orchestration` (`Proxy`) + `Health` на `:50051`.
- Собственный `Router` (метод + путь → handler) поверх `ProxyRequest`:
  маршруты `POST /auth/anonymousSignIn`, `GET /auth/getMe`, `POST /auth/refreshTokens`,
  `POST /auth/verify`, `GET /profile/getMe`, `GET /profile/get`.
- gRPC-клиенты `auth` и `profile` (через `@shared/grpc-client-manager`).
- Транспортная авторизация: middleware `jwt-from-proxy-request`, `authorizationRequired` у маршрута.
- Prisma-подключение с `check()` (схема без моделей — используется только health-проверка).
- Health-использование db/message/job.

## Non-responsibility

- Приём HTTP-трафика и конвертация HTTP ↔ gRPC (это gateway) — orchestration получает
  уже готовый `ProxyRequest`. Также не его зона: бизнес-логика профилей и
  аутентификации (→ profile / auth), транспортные слои shared (→ shared).

## Dependencies

`@shared/{logger,errors,grpc-client-manager,kafka-manager,pg-boss-manager}`,
`@prisma/client`, `ioredis`, `jsonwebtoken`, `bcrypt`.

## Dependency direction

Как в родителе. Специфика: `domain/` содержит только value-objects (сущностей и
интерфейсов репозиториев нет); use-cases `auth/*` и `profile/*` — тонкие обёртки над
gRPC-клиентами; контроллеры `infrastructure/proxy/controllers/*` импортируют use-cases.

## Entry points

- gRPC `:50051` — единственный вход (HTTP-сервера нет).
- `src/app.ts`.

## Structure

- `src/infrastructure/proxy/` — `router.ts`, `controllers/{auth,profile}.controller.ts`,
  `routes/{auth,profile}.routes.ts`, `middlewares/`, `adapter/response.ts`,
  `utils/{parse-body,parse-query-params,jwt-from-proxy-request}.ts`.
- `src/infrastructure/grpc/clients/` — `auth.client.ts`, `profile.client.ts`,
  `engine.client.ts` (клиент gusteau объявлен, но не используется).
- `src/application/use-cases/{auth,profile,health}/`.
- `src/infrastructure/cache/` — Redis `CacheService` **не подключён** (не создаётся в
  `build-application.ts`, конфиг пуст).
- `src/infrastructure/db/prisma/schema.prisma` — без моделей; тестов нет.

## Architectural rules

- Новый внешний путь → `infrastructure/proxy/routes/*` (регистрация в `Router`) +
  контроллер → use-case → gRPC-клиент.
- `SERVICE_AUTH_URL`, `SERVICE_PROFILE_URL`, `SERVICE_GUSTEAU_URL` — адреса клиентов.
- Kafka/pg-boss присутствуют только для health: продюсеров/консьюмеров нет.

## Important files

- `src/infrastructure/proxy/router.ts` — маршрутизация и флаги авторизации.
- `src/composition/build-application.ts` — wiring всех клиентов и use-cases.

## Navigation

- Поведение прокси на стороне-отправителе → [../gateway/](../gateway/CONTEXT.md).
- Маршруты аутентификации → use-cases в этом сервисе + gRPC-поверхность [../auth/](../auth/CONTEXT.md).
