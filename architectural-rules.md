# Architectural rules

- Весь прикладной код живёт в подмодулях (`services/*`, `shared/*`, `proto/`); на корне — только конфигурация и инструменты.
- `docker-compose.yml` и `.env` генерируются из `.dist`-шаблонов и не коммитятся (`.gitignore`).
- `@shared/*` всегда резолвятся как workspace-модули: overrides только в `pnpm-workspace.yaml` корня (overrides внутри подмодулей игнорируются).
- Код из proto генерируется исключительно через `make proto-generate`; результат живёт в потребителях (`services/<svc>/src/contracts/proto`, `services/front/lib/contracts/proto`).
- Состав списков сервисов — только в `make/parameters.mk` (`NODE_SERVICES`, `PRISMA_SERVICES`, `FLUTTER_SERVICES`, `GIT_SERVICES`); новые сервисы добавляются там.
- CI: сборка образов и деплой запускаются по `repository_dispatch`; ветки `dev` → `uat` → `prod` промотятся автоматически (push в `dev` → merge в `uat`, push в `uat` → merge в `prod`).
- Деплой-образы версируются тегом из `*_VERSION` в `.env`; образы — `ghcr.io/beartrando/lenvora-<service>/service`.
- Git-операции по всем репозиториям монорепы — через цели `make/git.mk` (единая ветка `dev`).
