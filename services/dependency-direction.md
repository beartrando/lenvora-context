# Dependency direction

Направление зависимостей (код →):

- `services/*` → [shared](../shared/purpose.md) (общие библиотеки), `services/*` → [proto](../proto/purpose.md) (сгенерированные контракты).
- [shared](../shared/purpose.md) и [proto](../proto/purpose.md) не зависят от сервисов.
- Вызовы между сервисами (по gRPC, направление «клиент → сервер»):
  - [gateway](gateway/purpose.md) → [orchestration](orchestration/purpose.md);
  - [orchestration](orchestration/purpose.md) → [auth](auth/purpose.md), [profile](profile/purpose.md), [gusteau](gusteau/purpose.md);
  - [streaming](streaming/purpose.md) → [profile](profile/purpose.md), [speech-to-text](speech-to-text/purpose.md).
- События Kafka: единственный связанный поток — [auth](auth/purpose.md) → `user.created` → [profile](profile/purpose.md) (единственный зарегистрированный consumer во всех сервисах). `profile.created` публикуется, но потребителя нет; `*.event-fired` объявлены только в `.env`, в коде не подключены (конфиги тем пусты), `bot.connecting-request` — наоборот, только в коде (`producers.config.ts` streaming, фактических публикаций нет).
- Обратные зависимости (auth/profile не зовут gateway и orchestration) отсутствуют; сервисы не импортируют код друг друга — только protobuf-контракты.
- Фронтенд [front](front/purpose.md) обращается к API только через [gateway](gateway/purpose.md).
