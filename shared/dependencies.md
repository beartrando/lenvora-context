# Dependencies

Корневой `pnpm-workspace.yaml`: `packages: ['services/*', 'shared/*']` и `overrides` всех `@shared/*` → `workspace:*` (локальный workspace приоритетнее `git+https`-спецификаций в lockfile'ах сабмодулей).

| Библиотека | runtime | dev |
|---|---|---|
| [logger](logger/purpose.md) | — | typescript |
| [errors](errors/purpose.md) | @bufbuild/protobuf | typescript, ts-proto |
| [kafka-manager](kafka-manager/purpose.md) | kafkajs, @shared/logger | typescript |
| [grpc-client-manager](grpc-client-manager/purpose.md) | @grpc/grpc-js, @shared/logger | typescript, @types/node |
| [pg-boss-manager](pg-boss-manager/purpose.md) | pg-boss, @shared/logger, @shared/kafka-manager | typescript, @types/node |

`errors` генерирует контракт из `proto/common/error.proto` (submodule [proto](../proto/purpose.md)): скрипт `proto:generate` (protoc + ts-proto), путь по умолчанию `../proto`, переопределяется `PROTO_DIR`.

Сборка: turbo `build` с `dependsOn: ["^build"]` — библиотеки компилируются раньше сервисов; `prepare` в `package.json` дополнительно собирает пакет при установке.
