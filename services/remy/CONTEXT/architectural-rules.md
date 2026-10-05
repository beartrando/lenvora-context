# Architectural rules

- Контракт — источник истины в [../../proto/remy.proto](../../../proto/remy.proto):
  `rpc Process(RemyRequest{text}) returns (RemyResponse{oneof: text|error})`.
  Правки контракта требуют `pnpm --filter remy proto:generate`.
- Порядок итерации по контракту: proto → `proto:generate` → use-case → handler →
  регистрация в `grpc/service.ts` → wiring в `composition` (по шаблону gusteau).
- `tsc --noEmit` и `pnpm --filter remy build` — зелёные (0 ошибок, проверено).
  ВНИМАНИЕ: раньше синтаксическая ошибка в одном файле (`ai/service.ts`) маскировала
  все остальные ошибки диагностики — при подозрительно чистом выводе проверять осознанно.
- Тесты — колокейтед, `*.spec.ts` рядом с кодом: `src/application/use-cases/process/
  process.spec.ts` (юнит, без зависимостей) и `src/infrastructure/grpc/handlers/
  remy.handler.spec.ts` (реальный gRPC на localhost:50051). Полный набор — изнутри
  контейнера: `docker compose exec -T -w /usr/src/app/services/remy remy pnpm test`
  (порт 50051 на хост не опубликован, функциональный спека с хоста не подключится);
  с хоста работает только юнит: `pnpm --filter remy test -- src/application/use-cases/`.
- env: `OPENROUTER_API_KEY` (пуст в .env/.env.dist), `AI_BASE_URL`, `AI_MODEL` — заданы с дефолтами.
- `CONTEXT/` этого сервиса игнорируется git (`.gitignore`: `/CONTEXT/`).
- `seed`-скрипт в package.json указывает на несуществующий путь и `tsx` не в зависимостях (факт, не трогали).
