# Navigation

- Поднять проект с нуля → `Makefile`: `make install` → `make up` → `make healthloop`.
- Изменить/добавить сервис → [services/](services/purpose.md); состав списков — `make/parameters.mk`, топология — `docker-compose.yml.dist`.
- Изменить gRPC-контракт → [proto/](proto/purpose.md) → `make proto-generate`.
- Изменить общую библиотеку → [shared/](shared/purpose.md).
- Настроить деплой/CI → `.github/workflows/*`, версии образов → `.env.dist`.
- Миграции/сиды/Prisma → `Makefile` (`migrate`, `seed`, `prisma-generate`).
- Git по всем репозиториям → `make/git.mk`.
- Процесс, git-workflow, lifecycle тикета → отдельный репозиторий wiki.
