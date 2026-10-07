# Responsibility

- Приём WS-соединений на маршруте `/event`: токен из `Authorization: Bearer` или `?token=`, `profileId` из query, heartbeat ping/pong каждые 5 с, terminate по таймауту.
- Проверка JWT локально по `ACCESS_SECRET` и сверка профиля с пользователем через gRPC [profile](../profile/purpose.md) (`AuthorizeProfileUseCase`).
- Голосовая сессия: `VoiceStreamUseCase` лениво открывает поток `SpeechToText.Transcribe` на `audio_start`, прокидывает `audio_chunk`/`audio_stop`, отправляет клиенту `transcription`/`error`.
- Кодирование WS-сообщений: protobuf по умолчанию, значение `SocketEncoding` из контекста сокета.
- Инфраструктурная доступность: gRPC health `check/status/livez/readyz` (компоненты pg-boss, Kafka, WS), корректный старт/останов по SIGINT/SIGTERM.
- Мост pg-boss → Kafka для темы `bot.connecting-request` (`infrastructure/job/service.ts`).
