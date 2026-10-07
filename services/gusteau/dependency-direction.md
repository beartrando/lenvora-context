# Dependency direction

- Внутрь: `src/app.ts` → `composition/build-application.ts` → (`application`, `infrastructure`, `domain`).
- `application` зависит от `domain` (сущности и интерфейсы репозиториев); `infrastructure` реализует порты Prisma-репозиториями и подключает gRPC/Kafka/pg-boss.
- Направление наружу: только хранение в PostgreSQL и (потенциально) Kafka; исходящих gRPC-вызовов и HTTP нет.
- Входящий поток: вызывающие стороны шлют запрос в `Gusteau` на `gusteau:50051`; контракт задан [proto](../../proto/purpose.md).
- Front и gateway не зависят от gusteau напрямую в коде сервиса — gusteau знает только свой контракт.
