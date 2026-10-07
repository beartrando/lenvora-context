# Dependency direction

- Внутрь: `src/app.ts` → `composition/build-application.ts` → (`application`, `infrastructure`, `domain`).
- `application` зависит от `domain` и портов; `infrastructure` зависит от `application`/`domain` и от транспортов (grpc-js, ws, kafkajs, pg-boss, Prisma).
- Исключение из строгого слоя: `VoiceStreamUseCase` (application) напрямую импортирует infrastructure-клиент `SpeechToTextClient`.
- Направление наружу: streaming → [profile](../profile/purpose.md), [speech-to-text](../speech-to-text/purpose.md), Kafka, PostgreSQL.
- Обратный вход: только вызовы health по gRPC и WS-соединения фронта; данные приходят от speech-to-text по уже открытому потоку.
- Front зависит от streaming как от WebSocket-сервера (`STREAMING_HOST:STREAMING_PORT`), но не наоборот.
