# Responsibility

- `DomainError` (абстрактный, `readonly code: ErrorCode`) и наследники:
  `AccessDeniedError`, `InvalidAuthorizationError`, `InvalidSessionError`,
  `NotFoundError`, `ValidationError`, `TokenExpiredError`, `UnknownError`,
  `ServiceUnavailableError`, `InvalidResponseError`.
- Свой сгенерированный контракт `ErrorCode` (из `proto/common/error.proto`).
