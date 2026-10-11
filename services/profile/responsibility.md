# Responsibility

- gRPC-сервер `Profile` (`getOrCreate`, `get`, `getByUser`) + `Health`.
- Prisma-модели `Profile`, `ProfileAchievement`, `Achievement`, `ExperienceLevel` (БД `profile`).
- Kafka: consume `user.created` (→ `GetOrCreateUseCase`), produce `profile.created`
  (через pg-boss-мост).
- Верификация access-token: `getOrCreate` берёт `userId` из токена в metadata,
  а не из запроса.
