# Services — сервисы монорепозитория Lenvora infra

Каталог `services/` содержит 9 сервисов: 8 Node.js-сервисов и Flutter-приложение `front/`.

Каждый сервис — отдельный git-submodule со своим репозиторием, package.json, Dockerfile и `.env` (см. `.gitmodules` корня).

Плюс два каталога без кода сервисов: `postgres/` и `pgbouncer/` — конфиги/данные для docker-compose.

Уровень описывает общий шаблон Node-сервиса один раз; детали — в секциях [children](children.md).
