# @shared/kafka-manager — обёртка над kafkajs

## Purpose

Создание инстансов Kafka, гарантируемое создание топиков, продюсер и консьюмер
с JSON-сериализацией — единая точка доступа к Kafka для всех сервисов.

## Abstraction level

Лист дерева: нижний уровень [shared/](../CONTEXT.md).

## Parent

[../../shared/CONTEXT.md](../../shared/CONTEXT.md).

## Children

Нет (файлы `types/register/admin/producer/consumer` — структура одного пакета).

## Responsibility

- `getKafkaInstance(config)` / `destroyKafkaInstance(clientId)` — кэш инстансов по clientId
  (retry 10×300ms, requestTimeout 5000).
- `ensureTopic(kafka, topic)` — идемпотентное создание топика с ожиданием лидера.
- `createProducer(config)` → `KafkaProducer.send/disconnect`.
- `createConsumer(config, consumerConfig)` — подписка `fromBeginning`, ретрай
  `UNKNOWN_TOPIC_OR_PARTITION`, JSON-парсинг, обработка ошибок хендлера.
- `withAdmin` — обёртка админ-операций.

## Non-responsibility

- Моста pg-boss → Kafka (это [../pg-boss-manager/](../pg-boss-manager/CONTEXT.md),
  он потребляет `createProducer`), выбора топиков и групп (конфиги — в сервисах).

## Dependencies

`kafkajs`, [../logger/](../logger/CONTEXT.md).

## Dependency direction

kafka-manager → logger. Не зависит от сервисов и от proto. Потребители:
`services/*/src/infrastructure/message/service.ts` и pg-boss-manager.

## Entry points

`src/index.ts` — barrel (`types`, `register`, `admin`, `producer`, `consumer`).

## Structure

```
shared/kafka-manager/src/
├── types.ts      # KafkaConfig {clientId, brokers, groupId}, ConsumerConfig<T>
├── register.ts   # кэш инстансов Kafka по clientId
├── admin.ts      # withAdmin, ensureTopic (waitForLeader, in-flight промисы)
├── producer.ts   # KafkaProducer, createProducer
├── consumer.ts   # createConsumer с retry и eachMessage
└── index.ts
```

## Architectural rules

- Топик создаётся автоматически перед send/consumer — ручное управление топиками
  в сервисах не требуется.
- `TODO` в `admin.ts` (масштабирование через `listTopics`) — известный открытый вопрос.

## Important files

- `src/producer.ts`, `src/consumer.ts` — то, что используется сервисами напрямую.
- `src/admin.ts` — `ensureTopic`.

## Navigation

- Кто именно шлёт/слушает топики → конфиги `services/<svc>/src/infrastructure/message/configs/`.
