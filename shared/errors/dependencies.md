# Dependencies

- Runtime: `@bufbuild/protobuf ^2.11.0` — нужен сгенерированному контракту (`require("@bufbuild/protobuf/wire")`).
- Dev: `typescript ^5.9.3`, `ts-proto ^2.11.6` (генерация контракта).
- Источник контракта: `proto/common/error.proto` submodule'а [proto](../../proto/purpose.md) — скрипт `proto:generate`, путь по умолчанию `../proto`, переопределяется `PROTO_DIR=/path/to/proto`.
- Других `@shared/*` не зависит. Свой `pnpm-lock.yaml` есть; `pnpm-workspace.yaml` нет (пакет входит в workspace монорепы, потребители — через `workspace:*`).
