# Architectural rules

- Gateway не содержит бизнес-логики: health-маршруты обрабатываются локально, всё остальное прозрачно уходит в [orchestration](../../services/orchestration/purpose.md).
- Прокси передаёт запрос как есть: `method`, `path`, `body` (JSON → bytes), все `headers`, `queryParams`; заголовок `x-debug-json` переключает ответ в JSON, иначе — сырые байты с `content-type: application/x-protobuf`.
- Единственная зависимость по gRPC — `SERVICE_ORCHESTRATION_URL` (`microservicesConfig.orchestration.url`); каталог health-агрегатора строится из всех `SERVICE_*_URL`.
- Нет prisma/Kafka/pg-boss в коде (объявленная в package.json зависимость `pg-boss` не используется), поэтому сервис stateless.
- Маршруты health живут под префиксом `/gateway` (`HttpServerService`), прокси — catch-all после него.
- Ответы контроллеров оформляет `wrapper.ts`: 200 + JSON либо 500 `{error}` при исключении.
- CORS открыт (`origin: '*'`), `cookie-parser` в `server.service.ts` закомментирован.
