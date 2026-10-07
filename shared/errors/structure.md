# Structure

```text
errors/
├── src/
│   ├── index.ts                    — ре-экспорт 10 классов
│   ├── errors/
│   │   ├── domain.error.ts         — абстрактный DomainError
│   │   ├── access-denied.error.ts
│   │   ├── invalid-authorization.error.ts
│   │   ├── invalid-response.error.ts
│   │   ├── invalid-session.error.ts
│   │   ├── not-found.error.ts
│   │   ├── service-unavailable.error.ts
│   │   ├── token-expired.error.ts
│   │   ├── unknown.error.ts
│   │   └── validation-failed.error.ts
│   └── contracts/proto/common/error.ts  — генерируемый контракт (proto:generate)
├── dist/                           — tsc-выход, включая dist/contracts/...
├── package.json                    — скрипт proto:generate, deps @bufbuild/protobuf
├── tsconfig.json, README.md, pnpm-lock.yaml
```

`src/contracts/proto/common/error.ts` — сгенерированный файл; в текущем рабочем дереве в `src/` его нет (собран только `dist/contracts/...`), восстанавливается `pnpm proto:generate`.
