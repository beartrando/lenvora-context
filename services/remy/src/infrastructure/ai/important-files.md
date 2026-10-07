# Important files

- `service.ts` — фасад `AiService`, точка входа для application-слоя.
- `interfaces/ai-client.interface.ts` — порт, через который узел используется извне.
- `clients/create-ai-client.ts` — фабрика, место подмены провайдера.
- `clients/openrouter.ts` — единственное место с SDK `openai` (базовый URL + ключ + модель).
- `configs/openrouter.ts` — чтение env и значения по умолчанию.
- `service.spec.ts` — покрытие фасада (мок фабрики).
