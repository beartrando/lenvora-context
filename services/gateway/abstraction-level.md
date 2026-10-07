# Abstraction level

Секция описывает gateway как сервис целиком: HTTP-поверхность, прокси-контракт, health-агрегацию, env, wiring, тесты.

Формат gRPC-запроса `Proxy` описан в [proto](../../proto/purpose.md); детали роутинга внутри оркестратора — в секции [orchestration](../../services/orchestration/purpose.md).

Общий шаблон сервиса и compose-топология — уровень [services/](../purpose.md).
