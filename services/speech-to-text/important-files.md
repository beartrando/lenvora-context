# Important files

- `../../proto/speech-to-text.proto` — контракт bidi-потока, источник истины по сообщениям и формату ответа.
- `src/infrastructure/grpc/handlers/speech-to-text.handler.ts` — каркас bidi-хендлера (точка будущей реализации).
- `src/infrastructure/grpc/service.ts` — gRPC-сервер, пустая регистрация `SpeechToText` и рабочая регистрация `Health`.
- `src/composition/build-application.ts` — wiring сервиса.
- `src/infrastructure/grpc/streams/engine.stream.container.ts` — файл с несуществующими импортами, ломает `tsc` (TS2307) и продакшен-сборку.
- `src/infrastructure/message/configs/*.ts` — Kafka-конфиги без тем; `consumers.config.ts` и `producers.config.ts` пусты.
- `src/infrastructure/db/prisma/schema.prisma` — datasource без моделей.
- `.env.dist`, `Dockerfile`, `jest.config.js`, `tsconfig.json`.
