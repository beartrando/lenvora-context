# Front — Flutter-клиент

## Purpose

Кроссплатформенный клиент (Android/iOS/Web/Desktop, Flutter 3.9): интерфейс игры,
аутентификация, профиль, настройки, in-app обновления. Единственный потребитель
внешних API платформы (HTTP gateway, WebSocket streaming).

## Abstraction level

Приложение того же уровня, что и бэкенд-сервисы ([../CONTEXT.md](../CONTEXT.md)), но
**другой технологической архитектуры**: Dart/Flutter вместо Node/TypeScript, свой
репозиторий-сабмодуль, свой CI. Внутренние слои (`lib/*`) описаны ниже, а не в шаблоне
Node-сервиса.

## Parent

[../CONTEXT.md](../CONTEXT.md) — уровень приложений; шаблон Node-слоёв к front
**не применяется**.

## Children

Нет дочерних CONTEXT.md. Внутренняя структура — в `lib/` (см. Structure).

## Responsibility

- UI-экраны и роутинг (`lib/features/*`, `lib/router.dart`, `lib/app.dart`).
- Домен клиента (`lib/domain/{profile,setting}`).
- Инфраструктура клиента: HTTP к gateway (`GATEWAY_PORT`, по умолчанию 9090),
  WebSocket к streaming (`ws://host:port/<path>?token=...`), хранилища, auth,
  локализация, protobuf-контракты в Dart.
- Сборка и релиз: собственный `Makefile`, `android/gradlew`, GitHub Actions
  (`.github/workflows/deploy.yml` → Google Play internal/production).

## Non-responsibility

- Не содержит бэкенд-логики и не переиспользует `@shared/*`; контракты получает из
  proto через Dart-codegen (`protoc --dart_out=grpc:lib/contracts/proto`).

## Dependencies

- `proto/` — внешний репозиторий `lenvora-proto` подключается напрямую CI-клиента
  (codegen в `lib/contracts/proto`).
- `gateway:9090` (HTTP), `streaming:8080` (WebSocket) — рантайм-зависимости.
- pubspec: flutter_riverpod, http, web_socket_channel, protobuf, firebase_*,
  shared_preferences, flutter_secure_storage.

## Dependency direction

Клиент зависит от публичных контрактов платформы (proto → Dart, HTTP/WS эндпоинты).
Обратного направления нет: бэкенд не знает о front. Кодовая база не входит в pnpm-workspace.

## Entry points

- `lib/main.dart` → bootstrap → `runApp`; `env/generate.dart` (генерация env, `make env-generate`).
- Сборка: `flutter build`, `make android-release`.

## Structure

```
services/front/
├── lib/
│   ├── contracts/proto/     # сгенерированный Dart из proto/
│   ├── domain/              # profile, setting
│   ├── features/            # home, profile, setting, update (UI + feature-инфраструктура)
│   ├── infrastructure/      # app, auth, device, error, http, localization,
│   │                        # proto, storage, websocket
│   ├── shared/              # общие компоненты
│   └── main.dart, app.dart, router.dart, theme.dart, firebase_options.dart
├── env/                     # .env + generate.dart
├── web/ android/ ios/ linux/ macos/ windows/   # платформенные целы
├── assets/, test/, build/, .dart_tool/
├── Makefile, pubspec.yaml, firebase.json
└── .github/workflows/deploy.yml
```

## Architectural rules

- Правки клиента не требуют пересборки backend-пакетов; правки `proto/` требуют
  повторного Dart-codegen (см. корневой `make proto-generate`, `FLUTTER_SERVICES := front`).
- front не входит ни в docker-compose, ни в deploy-процесс backend-сервисов.

## Git workflow

- Коммиты делаем в ветку `feature` (не в `dev` напрямую).
- Пушим в `origin`: `git push origin feature`.
- Пул-реквест через `gh` из `feature` в `dev`:
  `gh pr create --base dev --head feature --title "..." --body "..."`.

## Important files

- `lib/infrastructure/http/config/http_config.dart` — адрес gateway.
- `lib/infrastructure/websocket/config/websocket_config.dart` — адрес streaming.
- `pubspec.yaml` — зависимости и платформы.
- `env/.env` — `GATEWAY_PORT=9090`.

## Navigation

- Изменение backend-контракта → сначала [../../proto/](../../proto/CONTEXT.md), затем codegen.
- Поведение HTTP-прокси → [../gateway/](../gateway/CONTEXT.md);
  события → [../streaming/](../streaming/CONTEXT.md).
