# Important files

- `proto/remy.proto` — контракт (правится в отдельном репо proto).
- `src/application/use-cases/process/process.ts` — точка подключения реального AI вместо эхо.
- `src/infrastructure/ai/clients/openrouter.ts` — единственный AI-клиент в проекте.
- `src/composition/build-application.ts` — единственная точка wiring.
