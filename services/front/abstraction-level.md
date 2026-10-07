# Abstraction level

Уровень клиента: что видит пользователь и как приложение говорит с бэкендом; серверные детали (gRPC, домены сервисов) скрыты за gateway-HTTP и proto-конвертерами.

Внутри: UI (widgets) → Riverpod-провайдеры → `features`-слои → `infrastructure` (транспорт, хранение) → `domain` (сущности и порты). Сгенерированный код (`lib/contracts/proto`, `lib/infrastructure/localization/generated`) вне смыслового слоя и вне analyzer.
