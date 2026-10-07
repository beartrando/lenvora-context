# Non-responsibility

Gateway не отвечает за:

- роутинг внутри API, JWT-авторизацию и вызовы бизнес-сервисов — это [orchestration](../../services/orchestration/purpose.md);
- хранение данных, Kafka, очереди и prisma-миграции — их у gateway нет;
- бизнес-логику аутентификации/профилей/игровых сервисов — [auth](../../services/auth/purpose.md), [profile](../../services/profile/purpose.md) и остальные;
- собственные health-компоненты (db/job/kafka): gateway агрегирует чужие отчёты и отдаёт их, а не проверяет себя;
- обработку cookies/sessions на HTTP-уровне (`cookie-parser` отключён);
- деплой и compose-топологию — [корень](../../purpose.md); общий шаблон сервиса — уровень [services/](../purpose.md);
- реализацию [shared](../../shared/purpose.md) и исходники [proto](../../proto/purpose.md).
