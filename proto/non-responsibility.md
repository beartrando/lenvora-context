# Non-responsibility

- Не хранит сгенерированный код (ts-proto / dart) — он живёт в потребителях: [services/](../services/purpose.md).
- Не содержит бизнес-логики и инфраструктуры сервисов — → [services/](../services/purpose.md).
- Не содержит npm-библиотек `@shared/*` — → [shared/](../shared/purpose.md).
- Не управляет сборкой, CI/CD и деплоем — → [корень](../purpose.md).
- Не управляет Kafka (топики, эксплуатация) — инструменты `make/kafka.mk` в [корне](../purpose.md); здесь только payload-сообщения событий.
