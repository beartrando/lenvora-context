# Important files

- `Makefile` — основные команды: `up`, `install`, `install-deploy`, `migrate`, `prisma-generate`, `seed`, `proto-generate`, `test`, `build`, `reset*`, `healthloop`.
- `make/parameters.mk` — списки сервисов и `PROJECT_PREFIX := lenvora`.
- `make/git.mk` — commit/push/pull по всем репозиториям (`GIT_SERVICES` + `GIT_EXTRA_REPOS`).
- `make/kafka.mk` — чтение топиков Kafka.
- `docker-compose.yml.dist` — dev-топология: 8 Node-сервисов + kafka, kafka-ui, postgres, pgbouncer, redis; сеть `backend`, volume'ы `postgres_data`, `kafka_data`; front в compose не входит.
- `docker-compose.yml.deploy.dist` — deploy: образы `${*_VERSION}`, env-файлы `services/.env.<service>`.
- `.env.dist` — теги образов `*_VERSION=dev`.
- `.github/workflows/` — `build-and-push`, `deploy-docker-compose`, `deploy-gke`, `deploy-ssh-k8s`, `cleanup-registry`, `prune-docker-artifacts`, `promote-dev-to-uat`, `promote-uat-to-prod`.
- `pnpm-workspace.yaml` — пакеты `services/*`, `shared/*`; overrides `@shared/*` → `workspace:*`.
- `turbo.json` — задачи `build` (`dist/**`), `dev` (persistent).
- `tsconfig.base.json` / `tsconfig.json` — общий строгий TS-базис (ES2022, NodeNext).
- `.gitmodules` — 17 подмодулей: 9 сервисов, 5 `shared/*`, `proto`, `context`, `wiki`.
