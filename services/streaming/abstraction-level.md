# Abstraction level

Сервисный узел: как сервис взаимодействует снаружи (WS, gRPC) и чем обеспечен, без деталей доменов остальных сервисов.

Внутри — слои DDD-стиля: домен (только VO), application (use-case'ы и порты), infrastructure (транспорты и клиенты), composition (wiring). Протокольные детали (кодирование, heartbeat, bidi-поток) скрыты за infrastructure-адаптерами.
