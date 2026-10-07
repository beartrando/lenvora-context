# Non-responsibility

Этот уровень не описывает:

- внутренности конкретных сервисов (use-cases, схемы, тесты) — это секции [children](children.md);
- код и устройство [shared](../shared/purpose.md) и [proto](../proto/purpose.md);
- сборку, docker-compose, Makefile и CI/CD — [корень](../purpose.md);
- долговременные знания проекта, git-workflow и lifecycle тикета — wiki-репозиторий;
- `postgres/` и `pgbouncer/` как отдельные узлы — это только конфиги/данные для docker-compose, не сервисы;
- процессы деплоя/миграций в рантайме и работу других агентов, пишущих соседние секции.
