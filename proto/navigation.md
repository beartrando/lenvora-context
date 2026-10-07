# Navigation

- Изменить контракт сервиса → `<service>.proto` → `make proto-generate` → обновить сервер в [services/](../services/purpose.md) и клиентов (генерация у них).
- Добавить общий тип → `common/*.proto`, подключить import'ом в нужный `<service>.proto`.
- Новый gRPC-сервис в проекте → новый `proto/<service>.proto` → [корень](../purpose.md): `make/parameters.mk` (списки сервисов), `docker-compose.yml.dist`.
- Где сгенерированный код → [services/](../services/purpose.md): `src/contracts/proto` (Node), `lib/contracts/proto` (front).
