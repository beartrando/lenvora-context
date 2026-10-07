# Non-responsibility

Profile не отвечает за:

- выдачу и верификацию сессий, пользователей и устройств — [auth](../../services/auth/purpose.md);
- HTTP-роутинг, авторизацию запросов клиента и оркестрацию вызовов — [gateway](../../services/gateway/purpose.md) и [orchestration](../../services/orchestration/purpose.md);
- игровую логику, нутрициацию, стриминг и AI — [gusteau](../../services/gusteau/purpose.md), [streaming](../../services/streaming/purpose.md), [remy](../../services/remy/purpose.md);
- публикацию `profile.created` без потребителя: топик создаётся, но в текущем коде его никто не читает;
- `Get`/`GetByUser` без проверки токена на стороне profile — авторизацию выполняет вызывающая сторона;
- Prisma-миграции других сервисов, инфраструктуру и compose — [корень](../../purpose.md);
- реализацию [shared](../../shared/purpose.md) и исходники [proto](../../proto/purpose.md).
