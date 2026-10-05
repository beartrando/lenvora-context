# Entry points

Нет — это библиотеки. Импорт: `import logger from "@shared/logger"` и т. п.
Точки сборки: `pnpm --filter @shared/<pkg> build` (в Docker — до сборки сервиса,
в порядке logger → grpc-client-manager → errors → kafka-manager → pg-boss-manager).
