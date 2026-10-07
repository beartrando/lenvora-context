# Structure

Flutter/Dart-проект (SDK ^3.9.2), feature-first:

- `lib/main.dart` — bootstrap: dotenv, Firebase, Riverpod-контейнер, `getOrCreateAccessToken`, `runApp`; `lib/app.dart` — `MaterialApp`, локализация, тема; `lib/router.dart` — карта маршрутов; `lib/theme.dart`, `lib/firebase_options.dart`.
- `lib/features` — `home` (UI), `profile` (api, providers, ui), `setting` (application, infrastructure, ui), `update` (`AppUpdateService`).
- `lib/domain` — сущности/репозитории/VO без Flutter-зависимостей (`profile`, `setting`).
- `lib/infrastructure` — `http` (клиенты, конфиг, исключения, провайдеры), `auth` (клиент, сервис, хранение токенов, VO), `websocket` (конфиг и коннект к streaming), `proto` (конвертеры), `storage`, `device`, `error` (Firebase-репортёр), `localization` (arb + сгенерированный `l10n`), `app`.
- `lib/contracts/proto` — сгенерированный Dart из [proto](../../proto/purpose.md) (`protoc --dart_out`); локально нет `speech_to_text` и в `streaming.pb.dart` отсутствуют `audio_start`/`audio_chunk` — контракты регенерируются `make proto-generate` и в CI.
- `lib/shared` — пусто (только `.keep`).
- `test/widget_test.dart` — единственный тест, заглушка.
- Платформенные каталоги: `android`, `ios`, `linux`, `macos`, `web`, `windows`.
