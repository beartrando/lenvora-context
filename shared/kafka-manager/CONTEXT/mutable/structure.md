# Structure

```
shared/kafka-manager/src/
├── types.ts      # KafkaConfig {clientId, brokers, groupId}, ConsumerConfig<T>
├── register.ts   # кэш инстансов Kafka по clientId
├── admin.ts      # withAdmin, ensureTopic (waitForLeader, in-flight промисы)
├── producer.ts   # KafkaProducer, createProducer
├── consumer.ts   # createConsumer с retry и eachMessage
└── index.ts
```
