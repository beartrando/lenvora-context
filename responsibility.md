# Responsibility

- Инициализация проекта: `make install`, `make install-deploy` — подмодули, `.env` сервисов, `docker-compose.yml` из `.dist`, proto-генерация, миграции.
- Запуск dev-стека: `make up` (`docker compose up` по `docker-compose.yml.dist`).
- Миграции, сиды, Prisma: `make migrate`, `make prod-migrate`, `make prisma-generate`, `make seed`.
- Генерация клиентского/серверного кода из proto-контрактов: `make proto-generate`.
- Сборка и тесты сервисов: `make build`, `make test`; сброс стека/БД: `make reset`, `reset-db`, `reset-kafka`.
- Git-операции по всем репозиториям монорепы: `make git-commit-all`, `git-push-all`, `git-pull-all` (`make/git.mk`).
- CI/CD: `.github/workflows/*` — сборка и пуш образов в GHCR по событию из репозитория сервиса, деплой docker-compose по событию (GKE — событие не отправляется, k8s — только вручную), очистка реестра, автоматический promote `dev` → `uat` → `prod`.
- Workspace: `pnpm-workspace.yaml` (`services/*`, `shared/*`, overrides `@shared/*` → `workspace:*`), `turbo.json`, `tsconfig.base.json`.
- Версии образов для деплоя: `.env.dist` → `.env` (`*_VERSION=dev`).
