# Architectural rules

- RPC регистрируется только после контракта: `proto/speech-to-text.proto` (сабмодуль
  proto, ветка `dev`) → `pnpm speech-to-text proto:generate`.
- Контракт — bidi-stream (`(stream) returns (stream)`), первый потоковый RPC в проекте;
  ошибки в потоке — `common.error.ErrorObject` в том же `oneof`, что и в unary-сервисах.
  Ответ потока всегда oneof: транскрипция либо ошибка; ответов вне oneof нет.
- Авторизация не реализуется: `audio_start` обязателен первым сообщением
  (чанк/стоп до него → `INVALID_SESSION` и закрытие потока), профиль приходит
  в `audio_start` plain-полем, проверяет его вызывающая сторона (streaming /
  orchestration). Повторный `audio_start` — новая реплика (буферы сбрасываются),
  смена `profile_id` в рамках потока запрещена.
- Провайдер (OpenAI) изолирован: `application/ports/transcriber.port.ts` +
  `infrastructure/stt/` с фабрикой `create-stt-client.ts`. Смена провайдера = новый
  файл в `providers/` + строка в фабрике + `STT_PROVIDER` в env; типы провайдера не
  выходят за пределы `infrastructure/stt/`.
- Хранения нет: репозитории, Prisma-модели и миграции не добавляются.
- Тесты — только коллокейтед `src/**/*.spec.ts` (`jest.config.js`: ts-jest +
  `tsconfig.test.json`); набор покрывает bidi-хендлер — валидации протокола,
  буферизацию, ошибки провайдера, shutdown broadcast.
- Расширение `ErrorCode` в `proto/common/error.proto` допускается по мере
  необходимости (волновой codegen: proto → `@shared/errors` → потребители).
