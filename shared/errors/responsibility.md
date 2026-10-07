# Responsibility

- Единый словарь доменных ошибок и их wire-кодов (`ErrorCode` из `common/error.proto`) для всех сервисов: `UNKNOWN=0`, `INVALID_SESSION=1`, `ACCESS_DENIED=2`, `NOT_FOUND=3`, `VALIDATION_FAILED=4`, `SERVICE_UNAVAILABLE=5`, `INVALID_RESPONSE=6`, `INVALID_AUTHORIZATION=7`, `TOKEN_EXPIRED=401`.
- `DomainError extends Error`: абстрактный `readonly code`, `name = имя класса`, `message` — сообщение фиксируется в классе либо передаётся в конструктор.
- Параметры конструкторов: без аргументов — `InvalidAuthorizationError()`, `InvalidSessionError()`, `TokenExpiredError()`; опциональный message — `AccessDeniedError(message?)`, `UnknownError(message?)`; обязательные — `NotFoundError(entity)`, `ValidationError(message)`, `InvalidResponseError(serviceName)`, `ServiceUnavailableError(serviceName?)`.
- Гарантия совпадения кодов с контрактом, который проходит по wire: класс проверяется компилятором против `errorProto.ErrorCode`.
- Имя публичного экспорта `ValidationError` соответствует коду `VALIDATION_FAILED` (сам класс в файле называется `ValidationFailedError`).
