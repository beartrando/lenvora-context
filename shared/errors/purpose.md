# errors — доменные ошибки с кодами proto-контракта common.error

`@shared/errors` — абстрактный `DomainError` и 9 конкретных ошибок, у каждой `readonly code` из сгенерированного enum `ErrorCode` proto-контракта `common/error.proto`.

Сервисы бросают их в application-слое, а infrastructure-обработчики (`services/*/src/infrastructure/grpc/handlers/callback.error.ts`) транслируют `DomainError.code` в ответ.

Контракт `src/contracts/proto/common/error.ts` генерируется скриптом `proto:generate` (protoc + ts-proto) из submodule [proto](../../proto/purpose.md) — вручную не правится.

Публичный экспорт — 10 именованных классов, default-экспорта нет.
