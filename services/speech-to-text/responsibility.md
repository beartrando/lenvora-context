# Responsibility

- Роль по контракту [proto](../../proto/purpose.md) `SpeechToText.Transcribe` (bidi): приём `audio_start`/`audio_chunk`/`audio_stop`, ответ в том же потоке (`Transcription` или `ErrorObject`). Фактически реализации нет: `SpeechToTextHandler` — пустая заглушка, `registerSpeechToTextService()` ничего не регистрирует.
- gRPC health `check/status/livez/readyz` с проверками pg-boss и Kafka — единственная зарегистрированная поверхность.
- Инфраструктурная инициализация: Kafka, pg-boss, конфиг приложения, старт/останов.
- Аутентификация на контракт не распространяется: принадлежность профиля проверяет вызывающая сторона (streaming при открытии WS-сессии).
- `src/infrastructure/grpc/streams/engine.stream.container.ts` — заготовка рассылки по потокам: импортирует несуществующие `../types/engine.stream` и `contracts/proto/engine`, поэтому сборка сервиса не проходит (2 ошибки TS2307).
