# Important files

Корень репозитория:

- `pnpm-workspace.yaml` — `packages: ['services/*', 'shared/*']`, `overrides` всех `@shared/*` → `workspace:*`, `injectWorkspacePackages: true`.
- `.gitmodules` — 5 записей submodule: `shared/logger`, `shared/errors`, `shared/kafka-manager`, `shared/grpc-client-manager`, `shared/pg-boss-manager` (каждый на свой GitHub-репозиторий `beartrando/<name>`).
- `turbo.json` — задачи `build` (`dependsOn: ["^build"]`, `outputs: ["dist/**"]`) и `dev`.
- `tsconfig.base.json` — общие опции компиляции; tsconfig библиотек повторяют их + `rootDir`/`outDir`.
- `Makefile`, `docker-compose.yml.dist` — запуск сервисов (библиотеки отдельно не запускаются).

Внутри `shared/*`: `package.json` (экспорт-карта и скрипты), `src/index.ts`, `README.md` (порядок релиза), а у kafka-manager/grpc-client-manager/pg-boss-manager — собственный `pnpm-workspace.yaml` с fallback-оверрайдами на `git+https`.
