# Entry points

`@shared/kafka-manager` → `exports["."]`:

- Функции: `createProducer(config: KafkaConfig): Promise<KafkaProducer>`, `createConsumer<T>(config, consumerConfig): Promise<Consumer>`, `getKafkaInstance(config): Kafka`, `destroyKafkaInstance(clientId): void`, `withAdmin(kafka, fn)`, `ensureTopic(kafka, topic, partitions?)`, `waitForLeader(admin, topic, attempts?)`.
- Типы: `KafkaConfig` (`{ clientId, brokers, groupId }`), `ConsumerConfig<T>` (`{ topic, handler }`), `KafkaProducer` (`{ send(topic, message), disconnect() }`).

Тип `Consumer` возвращается как значение kafkajs — потребители работают с ним напрямую.
