# Structure

Маленький инфраструктурный узел из семи файлов:

- `service.ts` — фасад `AiService`: конструктор берёт `AiConfig`, создаёт клиент через фабрику, делегирует `chat`.
- `clients/create-ai-client.ts` — фабрика, возвращает `OpenRouterClient`.
- `clients/openrouter.ts` — `OpenRouterClient`: SDK `openai` с `apiKey` и `baseURL`, вызов `chat.completions.create`.
- `configs/openrouter.ts` — `aiConfig`, читает env.
- `interfaces/ai-client.interface.ts` — порт `AiClient` (`chat(messages): Promise<string>`).
- `interfaces/ai-config.interface.ts` — тип `AiConfig` `{ apiKey, baseUrl, model }`.
- `service.spec.ts` — юнит-тесты фасада (мок `createAiClient`).

Своих подкаталогов и зависимостей от домена сервиса нет; каталог не имеет других файлов.
