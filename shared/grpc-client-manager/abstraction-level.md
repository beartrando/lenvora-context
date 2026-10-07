# Abstraction level

Обёртка над одним unary-клиентом `@grpc/grpc-js`: не знает proto-контрактов, адресов и сервисов — только фабрику и колбэковый вызов.

Ниже — `@grpc/grpc-js` и [logger](../logger/purpose.md); выше — клиентская обёртка сервисов `services/*/src/infrastructure/grpc/clients/*.client.ts`, где создаётся фабрика `new XxxClient(url, grpc.credentials.createInsecure())`.
