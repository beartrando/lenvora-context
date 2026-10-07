# Architectural rules

- Зависимости только внутрь; wiring — исключительно `composition/build-application.ts`.
- Контракт: [proto](../../proto/purpose.md)/`remy.proto`, ответ только `oneof output { text | error }`; коды ошибок берутся из proto-контракта.
- LLM-провайдер доступен только через порт `AiClient` и фабрику `createAiClient`; `application`-слой не импортирует SDK.
- Секреты и параметры модели — в env (`OPENROUTER_API_KEY`, `AI_BASE_URL`, `AI_MODEL`), не в коде.
- В `ProcessUseCase` зафиксировано подключение AI через AI-порт (TODO в файле): эхо-поведение — текущее, не целевое.
- Тесты делятся на юнит (`process.spec.ts`, `ai/service.spec.ts`) и функциональный (`remy.handler.spec.ts` ходит в `localhost:50051`, запускается внутри контейнера через `make test`).
