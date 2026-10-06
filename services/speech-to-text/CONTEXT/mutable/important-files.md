# Important files

- `proto/speech-to-text.proto` — публичный контракт (сабмодуль proto).
- `src/infrastructure/grpc/service.ts` — `registerSpeechToTextService()` (сейчас пуст).
- `src/infrastructure/grpc/handlers/speech-to-text.handler.ts` — сейчас заглушка
  без зависимостей (чужие use-case'ы удалены), станет bidi-хендлером.
- `src/application/ports/transcriber.port.ts` и
  `src/infrastructure/stt/create-stt-client.ts` — граница провайдера.
- `src/composition/build-application.ts` — единственная точка DI.
- `jest.config.js` — привести к remy-стилю (коллокейтед тесты).
