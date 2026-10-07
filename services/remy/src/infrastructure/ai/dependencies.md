# Dependencies

- Пакет `openai` (только этот узел использует SDK в сервисе remy).
- Env remy: `OPENROUTER_API_KEY`, `AI_BASE_URL`, `AI_MODEL`.
- Собственные `interfaces/` (`AiClient`, `AiConfig`) — других внутренних зависимостей нет.

Не зависит от [proto](../../../../../proto/purpose.md), Prisma, Kafka, pg-boss и от домена remy. Наружу — HTTPS-запрос к `AI_BASE_URL` (OpenRouter-совместимый endpoint).
