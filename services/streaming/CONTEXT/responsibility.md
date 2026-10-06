# Responsibility

- WebSocket-сервер (`WEBSOCKET_PORT ?? 8080`) с heartbeat-циклом (5s ping).
- `ConnectionHandler` — аутентификация (`?token=` или Bearer), авторизация профиля
  (gRPC `profile.GetByUser`), кодирование (protobuf по умолчанию / JSON при `x-debug-json`),
  диспатч по путям (`/event`), обработка закрытия.
- Реестр `EventWebsocketsRegistry` на двух in-memory `StateStore`
  (socket → контекст, profileId → socket) и `EventWebsocketSender` (отправка `EventObject`).
- gRPC-сервер `Health` на `:50051`.
- Форвардинг аудио-чанков в `speech-to-text`: клиент шлёт чанки по WS → сервис открывает
  bidi-gRPC-поток в `SERVICE_SPEECH_TO_TEXT_URL` и первым сообщением передаёт
  `audio_start` (в поток инжектится `profile_id` авторизованной WS-сессии, JWT
  не передаётся — авторизация остаётся на этом сервисе), далее — чанки; финальная
  транскрипция из потока отправляется клиенту по WS (`speech-to-text` дополнительно
  публикует её в Kafka сам).
- Kafka: produce `bot.connecting-request` (единственный продюсер-топик, через pg-boss-мост).
