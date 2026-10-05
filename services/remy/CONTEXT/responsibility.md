# Responsibility

- gRPC-сервер на `:50051` с двумя сервисами: `Remy.Process` и `Health` (check/status/livez/readyz).
- `src/infrastructure/ai/` — `AiClient` интерфейс, `OpenRouterClient` (SDK `openai`),
  конфиг `OPENROUTER_API_KEY` / `AI_BASE_URL` / `AI_MODEL`. К wiring пока не подключён.
- Каркас приложения: lifecycle (`application.ts`: message → job → grpc),
  Prisma-подключение (моделей в схеме нет), auth, message/job-инфраструктура.
