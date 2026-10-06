# Responsibility

- `GrpcClientManager<TClient>`: конструктор принимает фабрику клиента, `call<TResult>(fn)` —
  до 2 попыток; на recoverable-ошибке закрывает и пересоздаёт клиент.
- `isRecoverableError`: `UNAVAILABLE`, `DEADLINE_EXCEEDED`, `RESOURCE_EXHAUSTED`,
  `ABORTED`, `INTERNAL` с «connection»/«timeout» в details.
- Типы `GrpcClientFactory<T>`, `GrpcCall<TClient, TResult>`.
- Только unary: потоковые (stream/bidi) вызовы пакет не обслуживает.
