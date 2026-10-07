# Structure

```text
kafka-manager/
├── src/
│   ├── index.ts      — ре-экспорт types, register, admin, producer, consumer
│   ├── types.ts      — KafkaConfig, ConsumerConfig<T>
│   ├── register.ts   — getKafkaInstance / destroyKafkaInstance (Map<clientId, Kafka>)
│   ├── admin.ts      — withAdmin, ensureTopic, waitForLeader
│   ├── producer.ts   — createProducer, тип KafkaProducer
│   └── consumer.ts   — createConsumer
├── dist/             — tsc-выход
├── package.json      — deps: kafkajs, @shared/logger
├── tsconfig.json, README.md
├── pnpm-workspace.yaml, pnpm-lock.yaml   — автономная установка вне монорепы
```

Весь контракт — 5 модулей; `src/internal/` и подмодулей нет.
