# Responsibility

- `call<TResult>(fn)`: до 2 попыток `execute(fn)`; если ошибка не восстановима — throw немедленно; после двух recoverable-неудач (с двумя reconnect'ами) выполняется финальный третий вызов — его результат или ошибка уходят наружу.
- `reconnect()`: `client.close?.()` (если клиент — `grpc.Client`) и повторное создание через фабрику `createClient`.
- `isRecoverableError()`: `grpc.ServiceError` с кодами `UNAVAILABLE`, `DEADLINE_EXCEEDED`, `RESOURCE_EXHAUSTED`, `ABORTED` либо `INTERNAL` c «connection»/«timeout» в `details`.
- Логирование через `@shared/logger`: `warn` при recoverable-ошибке и reconnect, `error` (`code`, `details`, `metadata`) при отказе вызова.
- Фабрика клиента (`GrpcClientFactory<T>`) приходит снаружи: адрес, credentials и proto-тип знает потребитель, менеджер — только жизненный цикл клиента.
