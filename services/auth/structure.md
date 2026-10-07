# Structure

```text
auth/src/
├── app.ts                                 # buildApplication().start()
├── composition/build-application.ts       # wiring: message, job, db, grpc
├── application/
│   ├── application.ts                     # lifecycle: message → job → db → grpc, SIGINT/SIGTERM
│   ├── ports/token-service.port.ts        # интерфейс генерации/верификации токенов
│   └── use-cases/auth/{anonymous-sign-in,refresh-tokens,logout,get-user}.ts (+ spec, кроме logout)
│                     /health/{health,status,livez,readyz}.ts
├── domain/                                # user, session, device: entities, repository, value-objects
├── infrastructure/
│   ├── app/config.ts                      # APP_NAME/APP_ENV/APP_VERSION
│   ├── auth/{config,service}.ts           # JWT (jsonwebtoken), обязателен ACCESS_SECRET + REFRESH_SECRET
│   ├── db/                                # DbService (transaction), prisma/schema.prisma + migrations, repositories, mappers, seed
│   ├── grpc/                              # service.ts, handlers/, helpers/auth.helper.ts, mappers/, config.ts (GRPC_PORT)
│   ├── job/                               # pg-boss + Kafka-bridge (producer-топики)
│   └── message/                           # Kafka: configs/{message,consumers,producers}, consumers пуст
└── contracts/proto/                       # сгенерированный код (в .gitignore)
```

Prisma-модели: `User`, `Session`, `Device` (таблицы `users`, `sessions`, `devices`), миграция `20260915160724`.
