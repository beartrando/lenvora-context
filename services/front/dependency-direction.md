# Dependency direction

- Внутрь: UI → Riverpod-провайдеры → `features` → `infrastructure` → `domain`; `domain` не зависит от Flutter и транспорта.
- Наружу: front → [gateway](../gateway/purpose.md) (HTTP), front → [streaming](../streaming/purpose.md) (WebSocket); ответ идёт в том же направлении запроса.
- Сгенерированные `lib/contracts/proto` зависят только от [proto](../../proto/purpose.md) и используются конвертерами `lib/infrastructure/proto/converters`.
- Front не вызывает gRPC-сервисы ([gusteau](../gusteau/purpose.md), [remy](../remy/purpose.md), [speech-to-text](../speech-to-text/purpose.md)) напрямую.
- Обратная зависимость bff-сервисов на front отсутствует: клиент ссылается на бэкенд по адресам из env.
