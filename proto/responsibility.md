# Responsibility

- Хранит gRPC-сервисы: `auth`, `gusteau`, `orchestration`, `profile`, `remy`, `speech-to-text`; `streaming.proto` — только message/enum (service-блока нет).
- Хранит общие типы контрактов: `empty`, `error`, `health`, `key_value`, `ping`, `response` (`common/`).
- Служит источником для генерации кода: ts-proto (Node-сервисы) и dart (Flutter front).
- Изменяется и пушится как отдельный репозиторий (см. `make/git.mk`, `GIT_EXTRA_REPOS`).
