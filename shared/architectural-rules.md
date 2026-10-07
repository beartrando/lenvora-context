# Architectural rules

- Библиотеки не импортируют код `services/*` и не подключают `proto/*` напрямую: `errors` получает контракт копией, сгенерированной скриптом, а не импортом.
- Зависимости между библиотеками направлены только вниз: `logger` ← `kafka-manager` / `grpc-client-manager` / `pg-boss-manager`; `kafka-manager` ← `pg-boss-manager`. Обратных связей нет.
- Публичный контракт библиотеки — только `exports["."]` из `src/index.ts`; внутренние модули (`src/internal/*` у pg-boss-manager) наружу не выдаются.
- Изменения `shared/*` выполняются в их submodule-репозиториях; в монорепу попадают как обновление указателя submodule.
- Новая библиотека в `shared/` повторяет общий шаблон (`package.json`, `tsconfig.json`, скрипты, README) и добавляется в `.gitmodules` и в `overrides` корневого `pnpm-workspace.yaml`.
- Версии зависимостей от `@shared/*` внутри `shared/` — `workspace:*` (в собственных lockfile'ах сабмодулей остаются `git+https…#v1.0.0`, в монорепе их перекрывает корневой override).
