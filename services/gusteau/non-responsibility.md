# Non-responsibility

- Не обслуживает HTTP и WebSocket — только внутренний gRPC; публичный доступ через [gateway](../gateway/purpose.md).
- Не аутентифицирует вызывающие стороны и не управляет токенами — это [auth](../auth/purpose.md).
- Не владеет профилем: `profile_id` — внешний ключ, данные профиля живут в [profile](../profile/purpose.md).
- Не рассчитывает планы питания и не генерирует тексты — это [remy](../remy/purpose.md).
- Не обрабатывает голос и события — producer'ом/consumer'ом Kafka не является: `producers.config.ts` и `consumers.config.ts` пусты, `addConsumer` не вызывается.
- Не отдаёт данные фронту напрямую (путь идёт через gateway/orchestration).
