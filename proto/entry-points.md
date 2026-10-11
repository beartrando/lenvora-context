# Entry points

- `make proto-generate` (корневой Makefile) — стирает и перегенерирует `src/contracts/proto`
  у всех `NODE_SERVICES`, `shared/errors` и делает Dart-codegen для `front`.
- `pnpm --filter <svc> proto:generate` — точечно для одного пакета.
