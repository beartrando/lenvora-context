# Architectural rules

- Новый use-case → `application/use-cases/<area>/`, он может импортировать domain,
  contracts и конкретный infrastructure (так принято фактически); регистрация —
  в `composition/build-application.ts` и в gRPC-хендлере/контроллере.
- Новый репозиторий → интерфейс в `domain/<area>/`, реализация в
  `infrastructure/db/repositories/`, маппер в `infrastructure/db/mappers/`.
- Новый RPC → правка `.proto` в `proto/` (отдельный submodule!) → `pnpm <svc> proto:generate`
  → хендлер в `infrastructure/grpc/handlers/`.
- Тесты — только коллокейтед `*.spec.ts` рядом с кодом (`testMatch: ['**/*.spec.ts']`,
  конфиги в стиле auth/gateway/profile/remy); отдельные тестовые деревья в проекте не
  используются. Фактически тесты есть в auth, gateway, profile, remy,
  speech-to-text, streaming; в gusteau остался
  нерабочий battle-тест (падает), в orchestration тестов нет.
- Compose монтирует в dev-контейнер `src`, `package.json`, `../../shared` и jest-конфиги
  remy — правки кода и конфигов подхватываются volume-ами без пересборки образа.
  Пересборка (`docker compose build`) не выполняется без явного указания пользователя.
