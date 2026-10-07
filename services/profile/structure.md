# Structure

```text
profile/src/
├── app.ts                                 # buildApplication().start()
├── composition/build-application.ts       # wiring + регистрация Kafka-consumer'а
├── application/
│   ├── application.ts                     # lifecycle: message → job → db → grpc
│   ├── ports/token-service.port.ts
│   └── use-cases/profile/{get-or-create,get,get-by-user}.ts (+ .spec.ts)
│                     /health/{health,status,livez,readyz}.ts
├── domain/profile/                        # ProfileEntity, ProfileRepository, value-objects
├── infrastructure/
│   ├── auth/{config,service}.ts           # только verifyAccessToken (ACCESS_SECRET)
│   ├── db/                                # DbService, prisma/schema.prisma + migrations,
│   │                                      # repositories/prisma-profile.repository.ts, mappers, seed/
│   ├── grpc/                              # service.ts, handlers/profile.handler.ts (+ spec),
│   │                                      # helpers/auth.helper.ts, mappers/profile.proto.mapper.ts, config.ts
│   ├── job/                               # pg-boss + Kafka-bridge
│   └── message/                           # configs + consumers/user-created.ts
└── contracts/proto/                       # сгенерированный код (в .gitignore)
```

Prisma-модели: `Profile` (nickname «guest», level 1, rating 1000, experience 0), `ProfileAchievement`, `Achievement`, `ExperienceLevel`; миграция `20260915160728`.
