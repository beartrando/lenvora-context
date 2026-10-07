# Abstraction level

Секция описывает orchestration как сервис целиком: контракт `Proxy`, таблицу маршрутов, авторизацию, вызовы gRPC-клиентов, env, wiring.

Детали самих сервисов [auth](../../services/auth/purpose.md) и [profile](../../services/profile/purpose.md) — в их секциях; формат `ProxyRequest/ProxyResponse` — в [proto](../../proto/purpose.md).

Шаблон сервиса и compose-топология — уровень [services/](../purpose.md).
