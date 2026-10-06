# Important files

- `common/error.proto` — коды ошибок, на которых построен `@shared/errors`.
- `common/health.proto` — контракт health-эндпоинтов всех сервисов и gateway-агрегатора.
- `gusteau.proto` (`EventObject`) — событийная модель для streaming.
- `speech-to-text.proto` — bidi-контракт распознавания: профиль передаётся в
  `audio_start` как plain-поле (авторизация не входит в контракт — её выполняет
  вызывающая сторона), ответ потока всегда oneof (транскрипция | ошибка).
