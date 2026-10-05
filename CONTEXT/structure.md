# Structure

```
├── services/            # приложения (см. services/CONTEXT/)
├── shared/              # библиотеки @shared/* (см. shared/CONTEXT/)
├── proto/               # контракты (git-submodule, см. proto/CONTEXT/)
├── make/                # фрагменты Makefile: parameters/kafka/front/terminal
├── .github/workflows/   # build-and-push, deploy-docker-compose, deploy-gke,
│                        # deploy-ssh-k8s (только manual), cleanup-registry, prune
├── Makefile             # install, up, reset, migrate, proto-generate, test, git-* …
├── docker-compose.yml   # dev-топология (рабочая копия; не в git)
├── docker-compose.yml.dist / .deploy.dist   # шаблоны dev и deploy
├── pnpm-workspace.yaml, turbo.json, tsconfig.base.json
└── .env.dist            # теги GHCR-образов для deploy-compose
```
