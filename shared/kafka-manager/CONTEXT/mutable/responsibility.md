# Responsibility

- `getKafkaInstance(config)` / `destroyKafkaInstance(clientId)` — кэш инстансов по clientId
  (retry 10×300ms, requestTimeout 5000).
- `ensureTopic(kafka, topic)` — идемпотентное создание топика с ожиданием лидера.
- `createProducer(config)` → `KafkaProducer.send/disconnect`.
- `createConsumer(config, consumerConfig)` — подписка `fromBeginning`, ретрай
  `UNKNOWN_TOPIC_OR_PARTITION`, JSON-парсинг, обработка ошибок хендлера.
- `withAdmin` — обёртка админ-операций.
