# Structure

Слои по шаблону родителя; специфика speech-to-text вместо gusteau-наследия — реализовано:

- `proto/speech-to-text.proto` (сабмодуль proto) → codegen в `src/contracts/proto/`.
- `src/domain/audio/`, `src/domain/transcription/` — VO (`audio-data`, `mime-type`,
  `language-code`, `confidence`, `transcription-id`) и entity `Transcription`;
  интерфейсов репозиториев нет.
- `src/application/ports/transcriber.port.ts` — единственный край к провайдеру;
  `use-cases/transcription/*` — валидация → порт → entity.
- `src/infrastructure/speech-to-text/` — `interfaces/`,
  `configs/speech-to-text.config.ts`, `providers/openai.ts`,
  `create-speech-to-text-client.ts`, `mappers/`, `service.ts`.
- `src/infrastructure/grpc/` — bidi-хендлер `handlers/speech-to-text.handler.ts`
  (профиль из `audio_start`, последовательная обработка сообщений, буферизация
  чанков, транскрипция на `audio_stop`, регистрация сессии в
  `streams/speech-to-text.stream.container.ts`), `mappers/transcription.proto.mapper.ts`,
  тип потока `types/speech-to-text.stream.ts`, регистрация RPC в `service.ts`.
- `src/infrastructure/{message,job}/` — produce события транскрипции через pg-boss-мост.
- Тесты — только коллокейтед `src/**/*.spec.ts` (конфиг jest: ts-jest +
  `tsconfig.test.json`); отдельное тестовое дерево с мёртвым battle-тестом
  (клон чужого проекта) удалено.
- Авторизация удалена: `infrastructure/auth/`, `domain/session/`,
  `application/ports/token-service.port.ts`, `ACCESS_SECRET` в `.env.dist`,
  мёртвый `infrastructure/message/config.ts` (наследие копипасты auth-сервиса).
- Удалённое gusteau-наследие (факт): `domain/{product,food-log,nutrition-coefficient}/`,
  `application/use-cases/{product,food-log,nutrition-coefficient}/`,
  `infrastructure/db/{repositories,mappers}/`, Prisma-модели и миграции
  (`schema.prisma` оставлен без моделей — генерация клиента и `migrate:deploy`
  из Dockerfile продолжают работать).
- Недостижимое наследие (кандидаты на чистку отдельно): `domain/{common,profile,user}`
  — entity `Profile`/`User` нигде не используются, тянут `UserId`.
