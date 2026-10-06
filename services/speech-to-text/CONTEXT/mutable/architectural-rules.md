# Architectural rules

- RPC регистрируется только после контракта: `proto/speech-to-text.proto` (сабмодуль
  proto, ветка `master`) → `pnpm speech-to-text proto:generate`.
- Контракт — bidi-stream (`(stream) returns (stream)`), первый потоковый RPC в проекте;
  ошибки в потоке — `common.error.ErrorObject` в том же `oneof`, что и в unary-сервисах.
- Авторизация ровно один раз — auth-сообщением при открытии потока; повторная проверка
  JWT на каждый чанк/Transcribe запрещена.
- Провайдер (OpenAI) изолирован: `application/ports/transcriber.port.ts` +
  `infrastructure/stt/` с фабрикой `create-stt-client.ts`. Смена провайдера = новый
  файл в `providers/` + строка в фабрике + `STT_PROVIDER` в env; типы провайдера не
  выходят за пределы `infrastructure/stt/`.
- Хранения нет: репозитории, Prisma-модели и миграции не добавляются.
- Тесты — только коллокейтед `src/**/*.spec.ts`; `jest.config.js` приводится к
  remy-образцу (`roots`, `testMatch: ['**/*.spec.ts']`, `moduleNameMapper` для
  `@shared/*`). Сейчас в сервисе нет ни одного теста, `pnpm test` падает
  с «no tests found».
- Расширение `ErrorCode` в `proto/common/error.proto` допускается по мере
  необходимости (волновой codegen: proto → `@shared/errors` → потребители).
