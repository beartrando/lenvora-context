# Dependency direction

- Внутрь: `AiService` → (`clients/create-ai-client` → `clients/openrouter`) → SDK `openai`; `configs/openrouter` → `interfaces/ai-config`.
- Узел зависит только от собственных `interfaces/` — ни от домена, ни от application-слоя remy.
- Наружу: исходящий HTTPS-вызов к `AI_BASE_URL`; ответ возвращается строкой вверх по вызову.
- Вход в узел: импорт `AiService` (или `createAiClient`) из application-слоя remy; сам узел ничего не импортирует из remy.
- Связь с gRPC-контрактом отсутствует: конвертация `RemyResponse` остаётся задачей `application`/`infrastructure/grpc`.
