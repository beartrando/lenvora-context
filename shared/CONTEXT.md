# Shared — переиспользуемые библиотеки

## Purpose

Уровень внутренних npm-библиотек `@shared/*`, потребляемых бэкенд-сервисами:
логирование, ошибки и обёртки над транспортом (Kafka, gRPC-клиенты, очереди pg-boss).

## Abstraction level

Ниже корня, выше отдельного пакета. Уровень-сосед `services/` (оба — дочерние корня),
но направление зависимостей между ними строго одно: **services → shared**.

## Parent

[../CONTEXT.md](../CONTEXT.md) — владеет workspace-конфигурацией (`shared/*` входит в
pnpm-workspace) и сборкой (`tsc` → `dist`, `main: dist/index.js`).

## Children

- [errors/](errors/CONTEXT.md) — иерархия `DomainError` и их proto-коды (общий словарь ошибок).
- [logger/](logger/CONTEXT.md) — минимальный консольный логгер с таймстампом и стеком.
- [kafka-manager/](kafka-manager/CONTEXT.md) — продюсер/консьюмер/топики поверх kafkajs.
- [grpc-client-manager/](grpc-client-manager/CONTEXT.md) — gRPC-клиент с ретраем и переподключением.
- [pg-boss-manager/](pg-boss-manager/CONTEXT.md) — очереди pg-boss, воркеры и мост pg-boss → Kafka.

## Responsibility

- Публичные API пакетов `@shared/*` и их внутренняя реализация.
- Директивы зависимостей **между** shared-пакетами (см. ниже).

## Non-responsibility

- Бизнес-логика сервисов, gRPC-серверы, Prisma, схемы БД — всё это живёт в `services/`.
- Определения proto-контрактов (→ [../proto/](../proto/CONTEXT.md)); `shared/errors`
  — единственный shared-пакет, содержащий сгенерированный контракт.

## Dependencies

Внутренние (фактические):

```
logger            ← kafka-manager, grpc-client-manager, pg-boss-manager
kafka-manager     ← pg-boss-manager (KafkaBridge использует createProducer)
errors            ← (никем внутри shared не зависит; зависит от proto/common/error.proto на codegen)
```

Внешние: `kafkajs`, `@grpc/grpc-js`, `pg-boss`, `@bufbuild/protobuf` (только errors).

## Dependency direction

- shared → shared (только перечисленные выше рёбра) и shared → `proto` (только codegen у errors).
- **shared никогда не импортирует сервисы** и не знает об их существовании.
- Сервисы → shared через `"@shared/<pkg>": "workspace:*"`; запрещено копирование кода
  shared в сервисы и обратное переиспользование кода сервисов в shared.

## Entry points

Нет — это библиотеки. Импорт: `import logger from "@shared/logger"` и т. п.
Точки сборки: `pnpm --filter @shared/<pkg> build` (в Docker — до сборки сервиса,
в порядке logger → grpc-client-manager → errors → kafka-manager → pg-boss-manager).

## Structure

```
shared/
├── errors/               # src/errors/*.error.ts + src/contracts/proto/common/error.ts (codegen)
├── logger/               # src/index.ts (один файл; src/ также содержит устаревшие .js/.d.ts)
├── kafka-manager/        # src/{types,register,admin,producer,consumer,index}.ts
├── grpc-client-manager/  # src/index.ts (GrpcClientManager)
└── pg-boss-manager/      # src/{index,types,pg-boss-manager}.ts + src/internal/*
```

## Architectural rules

- Каждый пакет собирается независимо (`dist/` в .gitignore — не коммитится);
  потребитель видит только `dist/index.js` + `index.d.ts`.
- Тестов и jest-конфигурации у shared-пакетов нет.
- Jest сервисов маппит `@shared/*` на исходники (`<rootDir>/../../shared/$1/src`) —
  правки shared видны в тестах сервисов без сборки.
- `shared/errors` генерируется своим скриптом `proto:generate`
  (`outputServices=none`, только `common/error.proto`).

## Important files

- `shared/pg-boss-manager/src/pg-boss-manager.ts` — самая объёмная библиотека уровня.
- `shared/errors/src/index.ts` — экспорт всех ошибок, используемых сервисами.

## Navigation

- Меняется контракт ошибок → [../proto/](../proto/CONTEXT.md) (`common/error.proto`)
  + [errors/](errors/CONTEXT.md).
- Меняется поведение Kafka-моста → [pg-boss-manager/](pg-boss-manager/CONTEXT.md).
