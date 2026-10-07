# Entry points

- Программный вход: `AiService` — `new AiService(aiConfig).process(text)`; альтернативно `createAiClient(config)` для прямой работы с портом `AiClient`.
- Env-вход: `OPENROUTER_API_KEY`, `AI_BASE_URL`, `AI_MODEL` (читаются в `configs/openrouter.ts` при импорте).
- Тестовый вход: `service.spec.ts` через `pnpm test` (в сервисе remy, jest + `jest.setup.ts`).
- Отдельного процесса, порта или CLI у узла нет; запуск — только в составе сервиса remy.
