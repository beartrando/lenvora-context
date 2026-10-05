# Architectural rules

- `EventHandler.handle()` сейчас пуст — отправка событий инициируется извне через
  `EventWebsocketSender`.
- Закомментированные заглушки: `grpc/handlers/{battle,profile}.service.ts`,
  `message/consumers/battle-started.ts` — остатки прошлой архитектуры, не рабочий код.
- env: `MESSAGE_PRODUCER_TOPIC_BOT_CONNECTING_REQUEST`, `MESSAGE_CONSUMER_EVENT_FIRED`
  (объявлен в `.env.dist`, но консьюмер не зарегистрирован).
