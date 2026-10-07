# Structure

```text
shared/
├── logger/               — @shared/logger
├── errors/               — @shared/errors
├── kafka-manager/        — @shared/kafka-manager
├── grpc-client-manager/  — @shared/grpc-client-manager
└── pg-boss-manager/      — @shared/pg-boss-manager
```

Каждый каталог — корень отдельного git-репозитория (submodule по `.gitmodules` корня) с одинаковой начинкой:

```text
<lib>/
├── src/            — исходники, index.ts — точка входа
├── dist/           — выход tsc (в .gitignore, артефакт сборки)
├── package.json    — пакет @shared/<name>, версия 1.0.0
├── tsconfig.json   — общий шаблон компиляции
├── README.md       — usage + порядок релиза
└── .gitignore      — node_modules, dist, *.tsbuildinfo
```

Отличия: у `kafka-manager`, `grpc-client-manager`, `pg-boss-manager` есть собственные `pnpm-workspace.yaml` и `pnpm-lock.yaml` (автономная установка вне монорепы, fallback на `git+https`); у `errors` — свой `pnpm-lock.yaml` и скрипт `proto:generate`; у `logger` зависимостей нет.
