# Important files

- `service.ts` — `AiService`: единственная операция `process(text)`.
- `clients/create-ai-client.ts` — фабрика, точка выбора провайдера.
- `clients/openrouter.ts` — реализация `AiClient` на SDK `openai`.
- `service.spec.ts` — юнит, фиксирующий контракт «конструктор → фабрика, process → chat».
