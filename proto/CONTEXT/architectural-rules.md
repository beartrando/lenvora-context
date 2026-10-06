# Architectural rules

- **Добавление RPC**: править файл в этом сабмодуле → закоммитить в `lenvora-proto`
  → перегенерировать потребителей (`pnpm <svc> proto:generate`); сгенерированный
  код живёт в `src/contracts/proto/` потребителя и **не коммитится** (закрыт
  `.gitignore` в `src/contracts/`), собирается локально/в CI.
- Сабмодули обновляются вручную/через Makefile и отдельным шагом CI
  (`build-and-push.yml`: «Update proto submodule»).
- Фактические несоответствия: `streaming.proto` не определяет сервис; сгенерированный
  набор в каждом сервисе **полный** (все определённые сервисы + common), хотя
  используется подмножество.
- `speech-to-text.proto` — контракт bidi-потока распознавания (первый stream/bidi
  RPC проекта): `audio_start` с `profile_id` → чанки → стоп, без авторизации;
  ошибки в потоке — `common.error.ErrorObject` в `oneof` ответа.
- Мёртвые ссылки на несуществующие контракты: `gateway/src/healthcheck.js`
  (импортирует `contracts/proto/gateway`), `battle.test.ts` в gusteau/speech-to-text
  (импортирует `contracts/proto/battle`).
