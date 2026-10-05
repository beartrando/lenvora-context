# Architectural rules

- `profile.created` публикуется без `await` (fire-and-forget) вне транзакции — факт.
- `GetOrCreateUseCase` — точка, где пересекаются Kafka-вход и Kafka-выход.
