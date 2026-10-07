# Abstraction level

Сервисный узел: внешний контракт (gRPC-операции домена питания) и способ хранения, без деталей чужих доменов.

Внутри — слои: `domain` (правила и порты), `application` (use-case'ы), `infrastructure` (Prisma, gRPC, Kafka, pg-boss), `composition` (wiring). Грань домена — Prisma-модели и proto-типы скрыты за мапперами.
