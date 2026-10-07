# Abstraction level

Сервисный узел: что сервис делает по контракту и как устроен изнутри, без деталей промптов, провайдеров и чужих доменов.

Уровни: `domain` (VO) → `application` (use-case'ы) ← `infrastructure` (gRPC, Kafka, pg-boss, Prisma, AI). AI-доступ вынесен в дочерний узел с собственным портом — прикладной код видит только `AiClient`/`AiService`.
