# Important files

- `proto/speech-to-text.proto` — публичный контракт (сабмодуль proto).
- `src/infrastructure/grpc/service.ts` — `registerSpeechToTextService()`
  (регистрирует bidi-RPC `transcribe`, `stop()` рассылает shutdown-уведомление).
- `src/infrastructure/grpc/handlers/speech-to-text.handler.ts` — bidi-хендлер:
  протокол `audio_start → chunk* → stop`, без авторизации (профиль — plain-поле
  `audio_start`); парная `speech-to-text.handler.spec.ts`.
- `src/application/ports/transcriber.port.ts` и
  `src/infrastructure/stt/create-stt-client.ts` — граница провайдера.
- `src/composition/build-application.ts` — единственная точка DI.
- `jest.config.js` — ts-jest + `tsconfig.test.json` (коллокейтед тесты).
