# Abstraction level

Инфраструктурная обёртка над сторонним клиентом `kafkajs`: скрывает кэш подключений, ретраи, создание топиков, ожидание лидера и JSON-сериализацию.

Ниже — `kafkajs` и [logger](../logger/purpose.md); выше — сервисные `MessageService` и `JobService` (`services/*/src/infrastructure/{message,job}/service.ts`), которые владеют конфигурацией и обработчиками.
