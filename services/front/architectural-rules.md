# Architectural rules

- `lib/contracts/proto` и `lib/infrastructure/localization/generated` — генерируемый код: вручную не правится, исключён из analyzer (`analysis_options.yaml`), обновляется `make proto-generate` (корень) и flutter_intl.
- `.env` не редактируется руками: генерируется скриптом `env/generate.dart` из `env/.env` + `env/.env.<окружение>` в корневые `.env*` (`make -C services/front env-generate`), в CI `.env` заменяется копией `.env.<ветка>`; читается при старте через `flutter_dotenv`.
- Состояние — Riverpod (`flutter_riverpod`); пакет `provider` объявлен в `pubspec.yaml`, но в коде не используется.
- Доступ к сети только через `HttpClient`/`AuthorizedHttpClient` и их провайдеры; доменные сущности не импортируют транспорт.
- Firebase — best-effort: сбой инициализации не блокирует запуск, репортёр подключается только на поддерживаемых платформах.
- Мобильные платформы в приоритете: иконки/сплэш настраиваются только для Android (`ios: false`), CI собирает Android App Bundle.
