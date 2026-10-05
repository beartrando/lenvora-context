# Architectural rules

- **Git-submodules**: `services/{auth,gateway,profile,remy,gusteau,front,orchestration,
  streaming,speech-to-text}` и `proto/` — отдельные репозитории; правки в них коммитятся
  в свои репозитории (`make git-commit-all`).
- **Топология dev-запуска** (сеть `backend`): gateway публикует `9090`, kafka `9092`,
  postgres `5432`; остальные сервисы внутри сети. Все сервисы зависят от `pgbouncer`
  (а auth/profile/orchestration/gusteau/streaming/speech-to-text — ещё от `kafka`).
- **Пересборка Docker-образов** (`docker compose build`) не выполняется без явного
  указания пользователя: dev-контейнеры получают код и конфиги через volume-монтирования
  docker-compose (`src`, `package.json`, jest-конфиги remy, `shared`) и пересоздаются
  через `docker compose up -d`.
- **Деплой**: активен путь docker-compose поверх SSH (`deploy-docker-compose.yml`,
  триггерится из `build-and-push.yml`). Kubernetes-манифестов в репозитории больше нет
  (директория `k8s/` удалена); workflows `deploy-ssh-k8s.yml` (manual) и `deploy-gke.yml`
  сохранены как legacy. `README.md` описывает старый план K3s — это план, не факт.
- **Codegen**: proto генерируется отдельно для каждого пакета (см. `proto**/CONTEXT`),
  не централизованно.
