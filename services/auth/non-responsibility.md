# Non-responsibility

Auth не отвечает за:

- профили, достижения и опыт — [profile](../../services/profile/purpose.md);
- HTTP-поверхность, роутинг и авторизацию запросов клиента — [gateway](../../services/gateway/purpose.md) и [orchestration](../../services/orchestration/purpose.md);
- health-агрегацию по всем сервисам — [gateway](../../services/gateway/purpose.md);
- пароли, e-mail-подтверждения и социальный вход: `passwordHash`/`googleId` есть в модели, кода входа нет, `GOOGLE_CLIENT_ID`/`GOOGLE_CLIENT_SECRET` в `.env` не используются;
- чтение событий из Kafka (consumers пуст) и какие-либо топики кроме `user.created`;
- инфраструктуру (postgres/pgbouncer/kafka), сборку и compose — [корень](../../purpose.md) и уровень [services/](../purpose.md);
- реализацию [shared](../../shared/purpose.md) и исходники protobuf-контрактов [proto](../../proto/purpose.md).
