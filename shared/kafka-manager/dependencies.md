# Dependencies

- Runtime: `kafkajs ^2.2.4`, `@shared/logger workspace:*`.
- Dev: `typescript ^5.9.3`.
- Собственный `pnpm-workspace.yaml` пакета: `overrides` `@shared/logger` → `git+https://github.com/beartrando/logger.git#v1.0.0` и `allowBuilds` — fallback для установки отдельно от монорепы; в монорепе действует корневой override `workspace:*` (см. [../](../purpose.md)).
- Свой `pnpm-lock.yaml`; `README.md` отмечает зависимость от `@shared/logger`.
