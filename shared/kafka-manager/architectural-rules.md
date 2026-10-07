# Architectural rules

- Публичный API — только через `src/index.ts`; типы `KafkaConfig` и `ConsumerConfig` импортируются отсюда и не дублируются в сервисах.
- Каждый `send` и `createConsumer` проходит через `ensureTopic` — топики не создаются вручную и не управляются outside библиотеки.
- Конфигурация приходит снаружи: библиотека не читает env; сервисы собирают `KafkaConfig` из `KAFKA_BROKERS` / `KAFKA_CLIENT_ID` / `KAFKA_GROUP_ID` (например, `services/*/src/infrastructure/message/configs/message.config.ts`).
- Зашитые ретраи/таймауты и JSON-формат общие для всех потребителей — менять только осознанно для всех сервисов сразу.
- Обработчик consumer'а не должен полагаться на повторную доставку: его исключение логируется, сообщение считается обработанным.
