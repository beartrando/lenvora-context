# Important files

- `src/register.ts` — кэш `Map<clientId, Kafka>` и параметры подключения (retry, requestTimeout).
- `src/admin.ts` — `ensureTopic`/`waitForLeader`/`withAdmin`: создание топика и ожидание лидера.
- `src/producer.ts` — `createProducer`: ensureTopic → send, JSON-сериализация, логирование ошибок.
- `src/consumer.ts` — `createConsumer`: подписка с ретраем, `CRASH`, обёртка `handler` с JSON.parse.
- `src/types.ts` — `KafkaConfig`, `ConsumerConfig<T>` (конфигурационный контракт сервисов).
- `src/index.ts` — публичные ре-экспорты.
- `package.json`, `pnpm-workspace.yaml`, `README.md` — версии, fallback-оверрайды, порядок релиза.
- Потребители: `services/*/src/infrastructure/message/service.ts`, `services/*/src/infrastructure/message/configs/message.config.ts`.
