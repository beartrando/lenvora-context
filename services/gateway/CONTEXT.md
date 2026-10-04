# Gateway — внешняя точка входа

## Purpose

Единственный сервис, принимающий HTTP-трафик извне: health-эндпоинты и проксирование
всех остальных запросов во внутреннюю сеть через gRPC `Orchestration.Proxy`.

## Abstraction level

Конкретный сервис внутри уровня [services/](../CONTEXT.md). Слои — по общему шаблону
родителя.

## Parent

[../CONTEXT.md](../CONTEXT.md).

## Children

Нет дочерних CONTEXT.md.

## Responsibility

- HTTP-сервер Express (`HTTP_PORT ?? 9090`): `/gateway/{health,full-health,status,livez,readyz}`,
  весь остальной трафик → `proxyController.proxy`.
- gRPC-сервер `Health` на `:50051`.
- `HealthAggregator` — health-фан-аут по всем сервисам из `SERVICE_*_URL`
  (remy, auth, profile, orchestration, gusteau, streaming).
- Конвертация HTTP-запроса в `ProxyRequest` и вызов gRPC `orchestration:50051`.

## Non-responsibility

- Ни бизнес-логики, ни БД, ни Kafka, ни pg-boss, ни доменного слоя — в gateway их нет.
- Маршрутизации внутренних вызовов (это orchestration), транспортной авторизации (orchestration).

## Dependencies

`@shared/{logger,errors,grpc-client-manager}`, `express`, `@grpc/grpc-js`.
Kafka/pg-boss/Prisma не используются (даже если объявлены в package.json).

## Dependency direction

Как в родителе. Специфика: слоя `domain/` и `application/ports/` не существует —
use-cases (`health`, `status`, `livez`, `readyz`) импортируют конкретный
`infrastructure/grpc/clients/health.aggregator`.

## Entry points

- HTTP `:9090` (единственный порт, публикуемый наружу в compose).
- gRPC `:50051` (только Health).
- `src/app.ts`.

## Structure

- `src/infrastructure/http/` — `server.service.ts`, `routes/health.routes.ts`,
  `controllers/{gateway,proxy}-controller.ts`, `configs/http.server.config.ts`.
- `src/infrastructure/grpc/clients/` — `health.aggregator.ts`, `orchestration.client.ts`.
- `src/infrastructure/orchestration/service.ts` — Request → `ProxyRequest`.
- `src/infrastructure/grpc/config/microservices.config.ts` — карта `SERVICE_*_URL`.
- `src/application/use-cases/` — health/status/livez/readyz; тесты `*.spec.ts`.

## Architectural rules

- Ответ прокси возвращается как `application/x-protobuf`; при заголовке `x-debug-json` — JSON.
- health-эндпоинты входят в `SERVER_ROUTES` (не проксируются).
- Мёртвый код: `src/healthcheck.js` (импортирует несуществующий `contracts/proto/gateway`),
  `request-logger.middleware.ts` не подключён.

## Git workflow

- Коммиты делаем в ветку `feature` (не в `dev` напрямую).
- Пушим в `origin`: `git push origin feature`.
- Пул-реквест через `gh` из `feature` в `dev`:
  `gh pr create --base dev --head feature --title "..." --body "..."`.

## Important files

- `src/infrastructure/http/server.service.ts` — маршруты и прокси-фоллбэк.
- `src/infrastructure/grpc/clients/health.aggregator.ts` — агрегация health.
- `.env.dist` — `HTTP_PORT`, `GRPC_PORT`, шесть `SERVICE_*_URL`.

## Navigation

- Поведение прокси на стороне-приёмнике → [../orchestration/](../orchestration/CONTEXT.md).
- Health-контракт → [../../proto/](../../proto/CONTEXT.md) (`common/health.proto`).
