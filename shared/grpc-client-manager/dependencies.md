# Dependencies

- Runtime: `@grpc/grpc-js ^1.14.3`, `@shared/logger workspace:*`.
- Dev: `typescript ^5.9.3`, `@types/node ^24.12.0`.
- Собственный `pnpm-workspace.yaml`: fallback-оверрайды `@shared/logger` → `git+https://github.com/beartrando/logger.git#v1.0.0` для установки вне монорепы; в монорепе действует корневой override `workspace:*` (см. [../](../purpose.md)).
- Свой `pnpm-lock.yaml`; proto-клиенты и контракты в зависимости не входят — их создаёт потребитель.
