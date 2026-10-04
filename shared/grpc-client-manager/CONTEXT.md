# @shared/grpc-client-manager — gRPC-клиент с ретраем

## Purpose

Дженерик-обёртка над `@grpc/grpc-js` клиентом: один вызов `call(fn)` с автоматическим
переподключением при восстанавливаемых ошибках и логированием.

## Abstraction level

Лист дерева: нижний уровень [shared/](../CONTEXT.md).

## Parent

[../../shared/CONTEXT.md](../../shared/CONTEXT.md).

## Children

Нет.

## Responsibility

- `GrpcClientManager<TClient>`: конструктор принимает фабрику клиента, `call<TResult>(fn)` —
  до 2 попыток; на recoverable-ошибке закрывает и пересоздаёт клиент.
- `isRecoverableError`: `UNAVAILABLE`, `DEADLINE_EXCEEDED`, `RESOURCE_EXHAUSTED`,
  `ABORTED`, `INTERNAL` с «connection»/«timeout» в details.
- Типы `GrpcClientFactory<T>`, `GrpcCall<TClient, TResult>`.

## Non-responsibility

- gRPC-**серверами** (это `infrastructure/grpc/service.ts` в сервисах), контрактами
  RPC (→ proto), выбором адресов и методов (конфиги сервисов).

## Dependencies

`@grpc/grpc-js`, [../logger/](../logger/CONTEXT.md).

## Dependency direction

grpc-client-manager → logger. Не зависит от сервисов и proto (генерированные клиенты
передаются потребителем из `contracts/proto/*`).

## Entry points

`src/index.ts` — единственный файл пакета.

## Structure

```
shared/grpc-client-manager/src/index.ts   # класс + типы + (повторяющееся объявление execute)
```

## Architectural rules

- Потребители: `gateway` (health-агрегатор, orchestration-клиент),
  `orchestration` (auth/profile/gusteau-клиенты), `streaming` (profile-клиент).
  Фактически не используется в `auth`, `profile` (заявлен в package.json, но не импортируется),
  `remy`, `gusteau`, `speech-to-text` (не заявлен).
- Фабрика клиента должна создаваться отдельно на каждый retry — ретрай спрятан внутри `call`.

## Important files

- `src/index.ts`.

## Navigation

- Конкретные вызовы gRPC-клиентов → `services/<svc>/src/infrastructure/grpc/clients/`.
