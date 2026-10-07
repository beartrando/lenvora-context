# Orchestration — gRPC-роутер Proxy: HTTP-поверхность API поверх auth и profile

Принимает от [gateway](../../services/gateway/purpose.md) gRPC-запрос `Proxy` (метод, путь, тело, заголовки, query) и отвечает HTTP-статусом с JSON-телом.

Владеет роутингом API: маршруты `/auth/*` и `/profile/*`, JWT-авторизация по `ACCESS_SECRET`, вызовы gRPC-клиентов `auth`, `profile` (клиент `gusteau` создан, но не задействован).

Композиция «anonymousSignIn → создание профиля» тоже живёт здесь: после входа автоматически вызывается `profile.GetOrCreate`.

Не хранит данных: Prisma-схема пуста (только datasource), Kafka/Redis-модули в wiring не подключены.
