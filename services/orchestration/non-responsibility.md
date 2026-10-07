# Non-responsibility

Orchestration не отвечает за:

- выдачу/верификацию JWT и хранение пользователей и сессий — [auth](../../services/auth/purpose.md);
- хранение и чтение профилей — [profile](../../services/profile/purpose.md);
- приём HTTP от клиента и публикацию порта — [gateway](../../services/gateway/purpose.md) собирает `ProxyRequest` и отдаёт ответ наружу;
- собственные данные: prisma-схема пуста, миграций нет, Redis-модуль (`infrastructure/cache`) и `proxy/middlewares` не подключены;
- вызовы [gusteau](../../services/gusteau/purpose.md) — клиент объявлен, но не используется;
- чтение событий Kafka и производство топиков (конфиги producer/consumer пусты);
- health-агрегацию по сервисам — это [gateway](../../services/gateway/purpose.md);
- сборку/compose — [корень](../../purpose.md); общий шаблон сервиса — уровень [services/](../purpose.md).
