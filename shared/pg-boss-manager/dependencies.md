# Dependencies

- Runtime: `pg-boss ^12.15.0`, `@shared/logger workspace:*`, `@shared/kafka-manager workspace:*`.
- Dev: `typescript ^5.9.3`, `@types/node ^24.12.0`.
- Собственный `pnpm-workspace.yaml`: fallback-оверрайды `@shared/logger` и `@shared/kafka-manager` → `git+https…#v1.0.0` для установки вне монорепы; в монорепе действует корневой override `workspace:*` (см. [../](../purpose.md)).
- Свой `pnpm-lock.yaml`. PostgreSQL — через окружение (`DATABASE_URL`), в зависимости не входит.
