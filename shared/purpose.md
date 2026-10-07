# shared/ — библиотеки @shared/* с инфраструктурной обвязкой для Node-сервисов

Каталог `shared/` содержит 5 TypeScript-библиотек: [logger/](logger/purpose.md), [errors/](errors/purpose.md), [kafka-manager/](kafka-manager/purpose.md), [grpc-client-manager/](grpc-client-manager/purpose.md), [pg-boss-manager/](pg-boss-manager/purpose.md).

Каждая — отдельный git-репозиторий, подключённый как submodule (записи в `.gitmodules` корня), с пакетом `@shared/<name>`.

В монорепе резолвятся как локальные workspace-модули: корневой `pnpm-workspace.yaml` объявляет `shared/*` workspace и переопределяет все `@shared/*` на `workspace:*`.

Задают единый шаблон: `src/` → `dist/` через `tsc`, единственная точка входа `exports["."]`, скрипты `build`/`dev`/`prepare`, версия `1.0.0`, README с порядком релиза.
