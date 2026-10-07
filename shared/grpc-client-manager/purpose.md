# grpc-client-manager — вызов gRPC-клиентов с reconnect и повторной попыткой

`@shared/grpc-client-manager` — класс `GrpcClientManager<TClient>`: держит один инстанс proto-клиента и выполняет колбэковый unary-вызов через `call(fn)`.

При восстановимой ошибке (`UNAVAILABLE`, `DEADLINE_EXCEEDED`, `RESOURCE_EXHAUSTED`, `ABORTED`, а также `INTERNAL` с «connection»/«timeout» в details) закрывает клиент, пересоздаёт его фабрикой и повторяет вызов — максимум 2 попытки.

Ошибки и reconnect'и логирует через `@shared/logger`; не-восстановимые ошибки пробрасываются сразу.

Потребители: gateway, orchestration, streaming (импорты в `src/`); auth и profile держат пакет в зависимостях без импортов.
