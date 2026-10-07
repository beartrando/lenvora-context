# Dependency direction

- profile → [shared](../../shared/purpose.md) и → [proto](../../proto/purpose.md) (сгенерированные `contracts/proto/profile.ts`, `auth.ts`).
- profile → PostgreSQL (своя БД `profile`); profile ⇄ Kafka: читает `user.created`, пишет `profile.created`.
- profile не вызывает другие сервисы по сети и не импортирует их код.
- Вызывающие profile: [orchestration](../../services/orchestration/purpose.md) (gRPC `ProfileClient`) и [streaming](../../services/streaming/purpose.md) (gRPC-клиент profile для авторизации профиля).
- Источник события `user.created` — [auth](../../services/auth/purpose.md); потребителей `profile.created` в текущем коде репозитория нет.
- Верификация токена идёт локально по общему `ACCESS_SECRET`, поэтому profile не зависит от auth в рантайме.
