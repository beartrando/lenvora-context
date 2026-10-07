# Architectural rules

- Зависимости только внутрь: `domain` ← `application` ← `infrastructure`; точка подключения — `composition/build-application.ts`.
- Контракт один: [proto](../../proto/purpose.md)/`gusteau.proto`; ответы — только `oneof output { объект | error }`, ошибки домена уходят через `callbackError`.
- Протокольные типы не попадают в домен: конвертация в `infrastructure/grpc/mappers/`, из домена в БД — в `infrastructure/db/mappers/`.
- Схема БД и миграции — `src/infrastructure/db/prisma/`; применяются `make migrate` / `pnpm migrate`, генерация клиента — `pnpm prisma:generate`.
- gRPC-хендлеры не аутентифицируют вызовы: `AuthService` (`ACCESS_SECRET`) в сервисе описан, но в wiring не подключён.
- Новые сценарии добавляются как use-case в `application` + регистрация в `build-application.ts`, без обращения из хендлера к репозиторию напрямую.
