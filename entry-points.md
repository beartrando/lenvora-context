# Entry points

- `make install` — первичная инициализация (dev), `make install-deploy` — для деплоя.
- `make up` — запуск dev-стека; далее `make migrate`, `make proto-generate`, `make build`, `make test`.
- `docker compose` — по сгенерированному `docker-compose.yml` (dev: порт gateway `9090`, gRPC-сервисы внутри на `50051`).
- CI/CD: push в сервисе шлёт `repository_dispatch: service-updated` → `build-and-push.yml` (сборка и пуш в GHCR) → событие `deploy-ssh-docker-compose` → `deploy-docker-compose.yml` (деплой, только ветка ≠ `dev`) и событие `cleanup-registry` → `cleanup-registry.yml`. `deploy-gke.yml` ждёт событие `deploy-gke`, которое сейчас ниоткуда не отправляется; `deploy-ssh-k8s.yml` и `prune-docker-artifacts.yml` — только вручную (`workflow_dispatch`).
- Health-чек стека: `make healthloop` → `http://127.0.0.1:9090/gateway/health`.
