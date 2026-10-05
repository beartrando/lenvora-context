# Architectural rules

- Новый use-case → `application/use-cases/<area>/`, он может импортировать domain,
  contracts и конкретный infrastructure (так принято фактически); регистрация —
  в `composition/build-application.ts` и в gRPC-хендлере/контроллере.
- Новый репозиторий → интерфейс в `domain/<area>/`, реализация в
  `infrastructure/db/repositories/`, маппер в `infrastructure/db/mappers/`.
- Новый RPC → правка `.proto` в `proto/` (отдельный submodule!) → `pnpm <svc> proto:generate`
  → хендлер в `infrastructure/grpc/handlers/`.
- Тесты: два фактических стиля — `*.spec.ts` рядом с кодом (auth, gateway, profile, remy;
  `testMatch: ['**/*.spec.ts']`) и `__tests__/**/*.test.ts` (gusteau, speech-to-text —
  в обоих нерабочая копия старого battle-теста; в orchestration, streaming тестов нет).
- Compose монтирует в dev-контейнер `src`, `package.json`, `../../shared` и jest-конфиги
  remy — правки кода и конфигов подхватываются volume-ами без пересборки образа.
  Пересборка (`docker compose build`) не выполняется без явного указания пользователя.
