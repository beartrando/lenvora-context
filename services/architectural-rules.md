# Architectural rules

- Единый шаблон: точка входа только `src/app.ts` → `composition/build-application.ts` → `Application.start()`; все зависимости собираются в composition root, DI-фреймворка нет.
- Слои: `domain` не знает об infrastructure; `application` (use-cases) зависит от интерфейсов домена и портов; `infrastructure` реализует порты; `contracts` — только сгенерированный protobuf-код.
- gRPC-контракты берутся только из [proto](../proto/purpose.md): `pnpm proto:generate` пишет в `src/contracts/proto/`, каталог в `.gitignore` и пересобирается в Docker (deps-стадия).
- Межсервисное общение — только gRPC (внутренняя сеть) и HTTP через gateway; напрямую чужой БД/чужие таблицы не читаются, у каждого сервиса свой DATABASE_URL.
- Health у всех сервисов одинаковый: `common.health.Health` (Check/Status/Livez/Readyz) из [proto](../proto/purpose.md).
- Аутентификация между сервисами — JWT (ACCESS_SECRET) в gRPC-metadata `authorization: Bearer ...`; секреты обязательны (`required()` в auth-config бросает при отсутствии).
- Kafka пишется через pg-boss bridge (`JobService.enqueue` → `pgBossManager.kafka.start`), а не напрямую producer'ом (исключение — health-пинг топика `healthcheck` в `MessageService.check`).
- Тесты — jest + ts-jest, `*.spec.ts` рядом с кодом; `__tests__` пуст у auth/profile/gateway/orchestration/streaming, у gusteau единственный файл `__tests__/functional/battle.test.ts` (не проходит), у remy/speech-to-text каталога нет; запуск `pnpm test` / `make test`.
- Миграции prisma выполняются только для сервисов из `PRISMA_SERVICES` (см. `make/parameters.mk`).
