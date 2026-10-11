# Architectural rules

- `EventHandler.handle()` сейчас пуст — отправка событий инициируется извне через
  `EventWebsocketSender`.
- Закомментированные заглушки: `grpc/handlers/{battle,profile}.service.ts`,
  `message/consumers/battle-started.ts` — остатки прошлой архитектуры, не рабочий код.
- env: `MESSAGE_PRODUCER_TOPIC_BOT_CONNECTING_REQUEST`, `MESSAGE_CONSUMER_EVENT_FIRED`
  (объявлен в `.env.dist`, но консьюмер не зарегистрирован).
- Чанковый форвардинг в `speech-to-text` — bidi-gRPC через отдельный клиент
  (`src/infrastructure/grpc/clients/speech-to-text.client.ts`), без ретрая:
  реконнект потока = потеря сессии, повторную авторизацию делает клиент.
- Тестов нет; при добавлении — только коллокейтед `src/**/*.spec.ts`.
