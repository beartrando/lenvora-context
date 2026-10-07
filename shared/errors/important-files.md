# Important files

- `src/index.ts` — публичные экспорт-точки (10 классов).
- `src/errors/domain.error.ts` — базовый абстрактный класс с `readonly code`.
- `src/errors/*.error.ts` — 9 реализаций, каждая связана с конкретным `ErrorCode`.
- `src/contracts/proto/common/error.ts` — генерируемый контракт (сейчас отсутствует в `src/`, восстанавливается `pnpm proto:generate`).
- `package.json` — скрипт `proto:generate` (protoc + ts-proto, `--proto_path=${PROTO_DIR:-../proto}`).
- `README.md` — порядок генерации контракта и релиза.
- `dist/contracts/proto/common/error.js` — собранный контракт (`ErrorCode`, `ErrorObject`).
- `proto/common/error.proto` (submodule [proto](../../proto/purpose.md)) — источник enum'а.
