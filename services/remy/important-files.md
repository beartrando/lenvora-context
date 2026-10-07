# Important files

- `src/composition/build-application.ts` — wiring сервиса (AI-сервис сюда пока не подключён).
- `src/application/use-cases/process/process.ts` — основной сценарий (эхо + валидация), рядом `.spec.ts`.
- `src/infrastructure/grpc/handlers/remy.handler.ts` — gRPC-хендлер, рядом функциональный `.spec.ts`.
- `src/infrastructure/grpc/service.ts` — регистрация сервисов `Remy` и `Health`.
- `src/infrastructure/ai/service.ts`, `clients/create-ai-client.ts`, `clients/openrouter.ts`, `configs/openrouter.ts` — AI-подсистема (см. дочерний узел).
- `src/infrastructure/db/prisma/schema.prisma` — datasource без моделей.
- `.env.dist` — перечень переменных, включая AI-ключ и модель; `jest.setup.ts`, `jest.config.js`, `Dockerfile`.
