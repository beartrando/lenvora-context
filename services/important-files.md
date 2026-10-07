# Important files

В корне (владелец — [корень](../purpose.md)):

- `docker-compose.yml` — топология сервисов, env_file, ports, healthcheck'и, volumes; `docker-compose.yml.dist` / `docker-compose.yml.deploy.dist` — шаблоны.
- `Makefile` + `make/parameters.mk` — списки `NODE_SERVICES`, `PRISMA_SERVICES`, `FLUTTER_SERVICES`; `make/kafka.mk` — просмотр топиков.
- `pnpm-workspace.yaml`, `turbo.json`, `tsconfig.base.json`, `.env.dist`, `.gitmodules` — workspace, сборка, версии сервисов, ссылки на submodule'ы.

На уровне `services/`:

- `services/.gitignore` — игнорирует `.env.*` и файлы контекста агента (`CONTEXT`).
- `services/<name>/package.json` — команды сервиса; `services/<name>/Dockerfile` — стадии сборки и healthcheck; `services/<name>/.env.dist` — проверенный состав переменных.
- `services/<name>/jest.config.js` — auth/profile/gateway/streaming/remy: маппинг `@shared/*` на `../../shared/*/src` и `testMatch` `**/*.spec.ts` с `roots: src`; у orchestration маппинг без `/src` и без `testMatch`; у gusteau/speech-to-text маппинга и `testMatch` нет (только transform ts-jest).
- `services/<name>/src/composition/build-application.ts` — фактический wiring зависимостей сервиса.
