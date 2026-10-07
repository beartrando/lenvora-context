# Responsibility

- Держать дерево `services/`: код сервисов, их Dockerfile, env-шаблоны, тесты, prisma-схемы.
- Описывать единый шаблон Node-сервиса (слои, composition root, gRPC/Prisma/Kafka/pg-boss, health) — один раз на этом уровне.
- Перечислять сервисы и их роли ([children](children.md)), маршруты чтения ([navigation](navigation.md)).
- Отличать сервисы от инфраструктурных каталогов: `postgres/`, `pgbouncer/` — только конфиги/данные для docker-compose.
- Фиксировать контрактные порты/топики/переменные окружения, проверенные по коду.
