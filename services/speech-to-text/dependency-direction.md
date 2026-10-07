# Dependency direction

- Внутрь: `src/app.ts` → `composition/build-application.ts` → (`application`, `infrastructure`, `domain`).
- `application` не зависит от gRPC/Kafka; `infrastructure` реализует транспорт и подключается к БД/Kafka.
- Направление вызова: [streaming](../streaming/purpose.md) → speech-to-text:50051 (bidi-поток `Transcribe`); сервис ни к кому сам не обращается.
- Наружу (будущее подключение AI) пойдёт по HTTPS от infrastructure-клиента провайдера; сейчас такого вызова нет.
- Kafka-обмен не задан: продюсеры и consumer'ы не регистрируются.
