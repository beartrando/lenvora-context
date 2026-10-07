# Dependency direction

- Зависит от: `proto/common/error.proto` (генерация копии контракта, не импорт репозитория), `@bufbuild/protobuf`; от других `@shared/*` не зависит.
- Зависятся от неё (проверено grep'ом по `services/*`) — все 8 Node-сервисов:
  - `DomainError` — в `services/*/src/infrastructure/grpc/handlers/callback.error.ts` (база для маппинга в ответ);
  - `TokenExpiredError` — в `services/*/src/infrastructure/auth/service.ts` (все, кроме gateway);
  - `InvalidAuthorizationError` — `infrastructure/grpc/helpers/auth.helper.ts` (profile, orchestration, streaming, auth);
  - `NotFoundError` — profile (handler, use-case, spec-тесты);
  - `ServiceUnavailableError`, `InvalidResponseError` — orchestration (proxy-контроллеры, adapter ответа);
  - в зависимостях `package.json` заявлен у всех 8 сервисов (`"@shared/errors": "workspace:*"`).
- Направление: `proto` → `errors` → `services`; обратных связей нет.
