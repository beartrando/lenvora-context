# kafka-manager — обёртка над kafkajs: producer, consumer, создание топиков

`@shared/kafka-manager` — фабрики `createProducer` / `createConsumer`, реестр инстансов `Kafka` по `clientId` и admin-хелперы `ensureTopic` / `withAdmin` / `waitForLeader`.

Конфигурация — `KafkaConfig { clientId, brokers, groupId }`; ретраи и таймауты зашиты: `retries: 10`, `initialRetryTime: 300` мс, `requestTimeout: 5000` мс.

Сообщения — только JSON (`JSON.stringify`/`JSON.parse`); топик создаётся автоматически (1 партиция, RF 1) перед отправкой и подпиской.

Потребляется 7 сервисами через `infrastructure/message/service.ts` (producer/consumer) и `infrastructure/job/service.ts` (тип `KafkaConfig` для моста pg-boss → Kafka).
