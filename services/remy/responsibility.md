# Responsibility

- gRPC-сервер на `:50051` с двумя сервисами: `Remy.Process` и `Health` (check/status/livez/readyz).
- `src/infrastructure/ai/` — `AiClient` интерфейс, `AiService`, `OpenRouterClient`
  (SDK `openai`); `AiConfig` передаётся в конструктор явно. К wiring пока не подключён.
- Каркас приложения: lifecycle (`application.ts`: message → job → grpc),
  Prisma-подключение (моделей в схеме нет), message/job-инфраструктура.
