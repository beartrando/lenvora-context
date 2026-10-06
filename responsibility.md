# Responsibility

- Pnpm workspace (`pnpm-workspace.yaml`: пакеты `services/*` и `shared/*`) и turbo-таски
  (`turbo.json`: `build`, `dev`).
- Общая TypeScript-база (`tsconfig.base.json`, без path-алиасов — `@shared/*` резолвится
  через workspace-линки `workspace:*`).
- Локальная разработка: `Makefile` + `make/*.mk` (`make/parameters.mk` — списки сервисов:
  `NODE_SERVICES`, `PRISMA_SERVICES`, `FLUTTER_SERVICES`), `docker-compose.yml`
  (рабочая копия, генерируется из `docker-compose.yml.dist`).
- Развёртывание: `docker-compose.yml.deploy.dist` (GHCR-образы, теги из `.env.dist`)
  и `.github/workflows/` (build → GHCR → деплой по SSH поверх docker-compose).
- Git-структура: сервисы и `proto/` — submodules (`.gitmodules`), `shared/` — обычная
  часть этого репозитория.
