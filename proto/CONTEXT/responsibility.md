# Responsibility

- Определения сервисов и сообщений: `auth.proto`, `profile.proto`, `orchestration.proto`,
  `gusteau.proto`, `streaming.proto`, `remy.proto` + `speech-to-text.proto` (bidi-поток
  распознавания: `audio_start` с `profile_id` → чанки → стоп, без авторизации) +
  общие `common/{error,empty,response,key_value,ping,health}.proto`.
- Версионирование контрактов (собственный репозиторий `lenvora-proto`, ветка `dev`).
