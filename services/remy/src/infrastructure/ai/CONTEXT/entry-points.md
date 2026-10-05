# Entry points

- `AiService.process(text: string): Promise<string>` — единственная публичная операция.
- `AiService` (конструктор с `AiConfig`) — создание через фабрику `createAiClient`.
- `createAiClient(config)` — фабричная функция, единственный способ получить клиент.

`AiConfig` (`{apiKey, baseUrl, model}`) передаётся в конструктор явно: конфига из
окружения в модуле больше нет (env-файл удалён как мёртвый).

Снаружи модуль пока не выведен: ни один файл вне `src/infrastructure/ai/` его не
импортирует; в `application/use-cases/process/process.ts` есть только TODO на подключение.
