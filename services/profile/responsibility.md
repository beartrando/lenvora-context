# Responsibility

- Создание профилей (GetOrCreate) — по gRPC-вызову и по Kafka-событию `user.created` (UserCreatedConsumer).
- Чтение профиля по id (`Get`) и по userId (`GetByUser`) с NotFoundError при отсутствии.
- Хранение профилей, достижений и таблицы уровней опыта: `profiles`, `profile_achievements`, `achievements`, `experience_levels`.
- Публикация события `profile.created` (через pg-boss bridge) при создании профиля.
- Проверка access-токена (`ACCESS_SECRET`) для метода `GetOrCreate`.
- gRPC-поверхность `Profile` + единый health.
