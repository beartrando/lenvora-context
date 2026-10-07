# Entry points

`@shared/errors` → `exports["."]` → `dist/index.js`. Именованные экспорты (10):

- `DomainError` — абстрактный базовый класс;
- `AccessDeniedError`, `InvalidAuthorizationError`, `InvalidSessionError`, `NotFoundError`, `ValidationError`, `TokenExpiredError`, `UnknownError`, `ServiceUnavailableError`, `InvalidResponseError`.

Default-экспорта нет. `ErrorCode` / `ErrorObject` / функции кодогенерации не публикуются — код доступен внутри классов как `error.code`.

Использование: `import { DomainError, TokenExpiredError } from "@shared/errors";`
