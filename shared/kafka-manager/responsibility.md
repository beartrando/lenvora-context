# Responsibility

- Реестр подключений: `getKafkaInstance(config)` отдаёт и кэширует `Kafka` в `Map` по `clientId`, `destroyKafkaInstance(clientId)` удаляет запись.
- Producer: `createProducer(config)` → `{ send(topic, message), disconnect() }`; `send` сначала `ensureTopic`, затем `producer.send` с JSON-значением (DefaultPartitioner); ошибки логируются через `@shared/logger` и пробрасываются.
- Consumer: `createConsumer(config, { topic, handler })` → `Consumer`; подписка `fromBeginning: true`, `groupId` из конфига; retry подписки при `UNKNOWN_TOPIC_OR_PARTITION` (пауза растёт до 3 с); событие `CRASH` логируется; исключение в `handler` логируется и не останавливает consumer (повторной доставки нет).
- Admin: `ensureTopic(kafka, topic, partitions=1)` — проверка `listTopics`, `createTopics`, ожидание лидера, дедупликация параллельных инициализаций через `Map<string, Promise>`; `waitForLeader` — до 30 попыток `fetchTopicMetadata` (пауза до 3 с), иначе throw; `withAdmin` — `connect`/`disconnect` вокруг колбэка.
- Внутренний логгер: `@shared/logger` (создание топика, ошибки отправки, старт consumer'а).
