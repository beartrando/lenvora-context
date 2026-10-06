# Structure

Слои по шаблону родителя; gusteau-специфика заменена на STT (целевое состояние —
пока не реализовано, ниже план):

- `proto/speech-to-text.proto` (сабмодуль proto) → codegen в `src/contracts/proto/`.
- `src/domain/audio/`, `src/domain/transcription/` — VO (`audio-data`, `mime-type`,
  `language-code`, `confidence`, `transcription-id`) и entity `Transcription`;
  интерфейсов репозиториев нет.
- `src/application/ports/transcriber.port.ts` — единственный край к провайдеру;
  `use-cases/transcription/*` — валидация → порт → entity.
- `src/infrastructure/stt/` — `interfaces/`, `configs/stt.config.ts`,
  `providers/openai.ts`, `create-stt-client.ts`, `mappers/`, `service.ts`.
- `src/infrastructure/grpc/` — bidi-хендлер `handlers/speech-to-text.handler.ts`,
  `mappers/transcription.proto.mapper.ts`, регистрация сервиса в `service.ts`.
- `src/infrastructure/{message,job}/` — produce события транскрипции через pg-boss-мост.
- Тесты — только коллокейтед `src/**/*.spec.ts` (конфиг jest в remy-стиле);
  отдельное тестовое дерево с мёртвым battle-тестом (клон чужого проекта) удалено.
- Удалённое gusteau-наследие (факт): `domain/{product,food-log,nutrition-coefficient}/`,
  `application/use-cases/{product,food-log,nutrition-coefficient}/`,
  `infrastructure/db/{repositories,mappers}/`, Prisma-модели и миграции
  (`schema.prisma` оставлен без моделей — генерация клиента и `migrate:deploy`
  из Dockerfile продолжают работать).
- Текущее состояние `src/`: `app.ts`, `composition/`, `application/{application.ts,
  use-cases/health/*}`, `infrastructure/{app,grpc,job,message,db,auth}` и
  `domain/{common,profile,session,user}` — из последних двух групп `auth/`,
  `domain/` и `application/ports/` сейчас недостижимы (кандидаты на чистку
  отдельно от gusteau-наследия); `handlers/speech-to-text.handler.ts` — пустая
  заглушка, `registerSpeechToTextService()` не регистрирует RPC.
