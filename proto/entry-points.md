# Entry points

Исполняемых точек входа нет — узел это данные (`.proto`).

Точки потребления:

- `make proto-generate` в [корне](../purpose.md) — очистка каталогов и генерация для всех сервисов;
- `pnpm --filter <service> proto:generate` — по одному Node-сервису (протокол: `protoc` + `ts_proto` → `src/contracts/proto`);
- `protoc --dart_out` — для front → `lib/contracts/proto`.
