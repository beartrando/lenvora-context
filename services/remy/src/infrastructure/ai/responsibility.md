# Responsibility

- Фасад `AiService.process(text)` — один user-сообщение → строка ответа модели.
- Выбор и создание провайдера: `createAiClient(config)` — единственная точка, где инстанцируется конкретный клиент (сейчас `OpenRouterClient`).
- Конфигурация из env: `OPENROUTER_API_KEY`, `AI_BASE_URL` (по умолчанию `https://openrouter.ai/api/v1`), `AI_MODEL` (по умолчанию `openrouter/free`).
- Работа с chat completions: маппинг ролей (`system|user|assistant`), извлечение `choices[0].message.content` (пустой ответ → пустая строка).
- Тестирование фасада: `service.spec.ts` с моком фабрики.
