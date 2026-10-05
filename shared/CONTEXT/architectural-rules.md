# Architectural rules

- Каждый пакет собирается независимо (`dist/` в .gitignore — не коммитится);
  потребитель видит только `dist/index.js` + `index.d.ts`.
- Тестов и jest-конфигурации у shared-пакетов нет.
- Jest сервисов маппит `@shared/*` на исходники (`<rootDir>/../../shared/$1/src`) —
  правки shared видны в тестах сервисов без сборки.
- `shared/errors` генерируется своим скриптом `proto:generate`
  (`outputServices=none`, только `common/error.proto`).
