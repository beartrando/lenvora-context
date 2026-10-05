# Children

- [auth/](../auth/CONTEXT/purpose.md) — идентичность: анонимный вход, сессии, токены; gRPC Auth.
- [profile/](../profile/CONTEXT/purpose.md) — профили игроков; gRPC Profile, Kafka user.created → profile.created.
- [gateway/](../gateway/CONTEXT/purpose.md) — единственная точка входа снаружи: HTTP → gRPC-прокси.
- [orchestration/](../orchestration/CONTEXT/purpose.md) — внутренний маршрутизатор HTTP-запросов
  поверх gRPC (Proxy).
- [streaming/](../streaming/CONTEXT/purpose.md) — WebSocket-сервер для событий в реальном времени.
- [gusteau/](../gusteau/CONTEXT/purpose.md) — кулинарный справочник: продукты, нутриенты, журнал еды; gRPC Gusteau.
- [remy/](../remy/CONTEXT/purpose.md) — AI-бот; gRPC Health, клиент OpenRouter (не подключён).
- [speech-to-text/](../speech-to-text/CONTEXT/purpose.md) — каркас сервиса распознавания речи (без реализации).
- [front/](../front/CONTEXT/purpose.md) — Flutter-клиент (mobile/desktop/web), отдельная архитектура.
sdf
Без CONTEXT/ (не являются сервисами): `postgres/`, `pgbouncer/` — только конфиги
и данные для docker-compose (`pgbouncer.ini`, `userlist.txt`, каталог данных БД).
