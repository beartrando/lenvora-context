# Entry points

- Код: `services/<name>/src/app.ts` → `composition/build-application.ts` → `application.start()`.
- Dev-запуск из compose: `pnpm dlx turbo run dev --parallel --filter <name>` (команда compose, перекрывает Dockerfile CMD `pnpm --filter <name> dev`), исходники монтируются bind-mount'ом.
- Прод-запуск: `node dist/app.js` (стадия `prod` Dockerfile), сборка `pnpm --filter <name> build`.
- Команды корня (`Makefile`): `make install` (сабмодули + копирование `.env` из `.env.dist`), `make up`, `make proto-generate`, `make migrate` / `make prisma-generate`, `make test`, `make build`, `make reset`.
- Порты (только из `.env`/конфигов): gRPC 50051 у всех Node-сервисов (`GRPC_PORT`), HTTP 9090 у [gateway](gateway/purpose.md) (`HTTP_PORT`), WebSocket 8080 у [streaming](streaming/purpose.md) (`WEBSOCKET_PORT`).
- Publish-ит корень только gateway `9090:9090` (плюс kafka 9092 и postgres 5432); остальные сервисы доступны только в сети `backend`.
- Docker healthcheck: для сервисов с gRPC — `nc -z localhost 50051` (Dockerfile) / `wget http://127.0.0.1:50051` (compose); для gateway — `curl http://localhost:9090/livez` (Dockerfile), фактический HTTP-маршрут health'а — `/gateway/livez`.
