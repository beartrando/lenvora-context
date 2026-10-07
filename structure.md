# Structure

```text
.
├── Makefile                     # команды (включает make/*.mk)
├── make/
│   ├── parameters.mk            # NODE_SERVICES, PRISMA_SERVICES, FLUTTER_SERVICES, GIT_SERVICES, PROJECT_PREFIX
│   ├── kafka.mk                 # kafka-list-topics, kafka-user-created-list
│   └── git.mk                   # git-commit-all / git-push-all / git-pull-all по всем репо
├── docker-compose.yml.dist      # dev-топология: сборка из исходников (target: dev)
├── docker-compose.yml.deploy.dist # deploy-топология: образы ghcr.io/beartrando/lenvora-*/service:${*_VERSION}
├── .env.dist                    # *_VERSION=dev (теги образов для deploy-compose)
├── .github/workflows/           # 8 workflow (см. important-files)
├── package.json                 # корневой пакет (turbo, ts-proto), packageManager pnpm
├── pnpm-workspace.yaml          # packages: services/*, shared/*; overrides @shared/* → workspace:*
├── pnpm-lock.yaml
├── turbo.json                   # задачи build, dev
├── tsconfig.base.json           # ES2022 / NodeNext / strict
├── tsconfig.json                # extends base, files: []
├── .gitmodules                  # подмодули
├── .dockerignore, .gitignore, README.md
├── services/                    # 9 сервисов (подмодули) + postgres/, pgbouncer/ (обычные каталоги)
├── shared/                      # 5 библиотек @shared/* (подмодули)
├── proto/                       # gRPC-контракты (подмодуль)
├── context/                     # рабочий контекст агента (подмодуль)
└── wiki/                        # долговременные знания (подмодуль, отдельный репозиторий)
```
