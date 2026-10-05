# Architectural rules

- Добавление нового типа ошибки: сначала `ErrorCode` в `proto/common/error.proto`
  (сабмодуль proto) → `pnpm --filter @shared/errors proto:generate` → новый класс
  → экспорт в `src/index.ts` → маппинг в хендлерах сервисов.
- Генерация выполняется и в Docker-стадии `deps` каждого сервиса, поэтому классы
  ошибок доступны сервисам без отдельной сборки пакета.
