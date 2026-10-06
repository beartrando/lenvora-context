# Dependencies

`@shared/{logger,errors,kafka-manager,pg-boss-manager}`, `jsonwebtoken`,
`@grpc/grpc-js` (сервер). `ws` заявлен, но не используется — WebSocket живёт в streaming.

- `@prisma/client` и БД: транскрипты не хранятся. Prisma-модели и миграции gusteau
  удалены (`schema.prisma` остался без моделей); зависимость, `prisma`-секция и запись
  `speech-to-text` в `PRISMA_SERVICES` (корневой `make/parameters.mk`) убираются
  при реализации — сейчас в `package.json`/`.env.dist`/Dockerfile ещё есть.
- `grpc-client-manager` не нужен: сервис не делает исходящих gRPC-вызовов.
- Kafka — только produce события транскрипции через pg-boss-мост
  (`MESSAGE_PRODUCER_...=speech-to-text.transcription-ready`).
