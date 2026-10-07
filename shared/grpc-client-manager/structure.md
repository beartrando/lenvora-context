# Structure

```text
grpc-client-manager/
├── src/
│   └── index.ts   — GrpcClientFactory, GrpcCall, GrpcClientManager (вся реализация, ~110 строк)
├── dist/          — tsc-выход
├── package.json   — deps: @grpc/grpc-js, @shared/logger
├── tsconfig.json, README.md
├── pnpm-workspace.yaml, pnpm-lock.yaml   — автономная установка вне монорепы
```

Один файл исходников, один класс; отдельных модулей нет.
