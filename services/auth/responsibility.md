# Responsibility

- Анонимная регистрация по `deviceId`: создание пользователя, устройства и сессии в одной Prisma-транзакции.
- Выдача и ротация пары access/refresh JWT (`ACCESS_SECRET` / `REFRESH_SECRET`), logout — удаление сессий пользователя.
- Хранение и чтение пользователей (`users`), сессий (`sessions`), устройств (`devices`).
- Публикация события `user.created` в Kafka при новом пользователе (через pg-boss bridge).
- gRPC-поверхность `Auth` + единый health (`Check/Status/Livez/Readyz`).
- Верификация собственных токенов для вызывающих сторон (metadata `authorization: Bearer …`).
