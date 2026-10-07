# Dependency direction

```text
services/*  →  @shared/*  →  { kafkajs, pg-boss, @grpc/grpc-js, @bufbuild/protobuf }
                                @shared/logger ← kafka-manager, grpc-client-manager, pg-boss-manager
                                @shared/logger ← @shared/kafka-manager ← @shared/pg-boss-manager
                                proto/common/error.proto ─генерация─→ @shared/errors
```

- Направление только `services → shared`: библиотеки не знают о сервисах и не могут от них зависеть.
- `logger` — нижний слой `shared/`, не зависит ни от одной библиотеки; `errors` зависит только от proto-контракта.
- Порядок сборки: `logger` → `kafka-manager`/`grpc-client-manager` → `pg-boss-manager` → сервисы (турбо `^build`).
- Потребители каждой библиотеки перечислены в её dependency-direction — факты проверены grep'ом по `services/*`.
