# Children

- [auth/](../auth**/CONTEXTpurpose.md) — идентичность: анонимный вход, сессии, токены; gRPC Auth.
- [profile/](../profile**/CONTEXTpurpose.md) — профили игроков; gRPC Profile, Kafka user.created → profile.created.
- [gateway/](../gateway**/CONTEXTpurpose.md) — единственная точка входа снаружи: HTTP → gRPC-прокси.
- [orchestration/](../orchestration**/CONTEXTpurpose.md) — внутренний маршрутизатор HTTP-запросов
  поверх gRPC (Proxy).
- [streaming/](../streaming**/CONTEXTpurpose.md) — WebSocket-сервер для событий в реальном времени.
- [gusteau/](../gusteau**/CONTEXTpurpose.md) — кулинарный справочник: продукты, нутриенты, журнал еды; gRPC Gusteau.
- [remy/](../remy**/CONTEXTpurpose.md) — AI-бот; gRPC Health, клиент OpenRouter (не подключён).
- [speech-to-text/](../speech-to-text**/CONTEXTpurpose.md) — распознавание речи: bidi-gRPC-поток
  аудио-чанков, провайдер OpenAI за интерфейсом, produce `transcription-ready` в Kafka;
  без хранения транскрипций и БД (каркас, реализация в работе).
- [front/](../front**/CONTEXTpurpose.md) — Flutter-клиент (mobile/desktop/web), отдельная архитектура.
sdf
Без CONTEXT/ (не являются сервисами): `postgres/`, `pgbouncer/` — только конфиги
и данные для docker-compose (`pgbouncer.ini`, `userlist.txt`, каталог данных БД).
