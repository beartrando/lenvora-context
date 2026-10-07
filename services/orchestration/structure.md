# Structure

```text
orchestration/src/
├── app.ts                                 # buildApplication().start()
├── composition/build-application.ts       # wiring: authService, message, job, db, controllers, router, grpc
├── application/
│   ├── application.ts                     # lifecycle: message → job → db → grpc
│   ├── ports/token-service.port.ts
│   └── use-cases/
│       ├── auth/{anonymous-sign-in,get-user,logout,refresh-tokens}.ts    # обёртки над AuthClient
│       ├── profile/{get-or-create-profile,get-profile,get-profile-by-user}.ts
│       └── health/{health,status,livez,readyz}.ts
├── domain/                                # только value-objects: Id, UserId, ProfileId, SessionAccessToken
├── infrastructure/
│   ├── auth/{config,service}.ts           # verifyAccessToken (ACCESS_SECRET)
│   ├── cache/                             # Redis-модуль (не подключён в composition)
│   ├── db/                                # DbService (health SELECT 1), prisma/schema.prisma без моделей
│   ├── grpc/
│   │   ├── config.ts                      # GRPC_PORT + SERVICE_AUTH_URL/SERVICE_PROFILE_URL/SERVICE_GUSTEAU_URL
│   │   ├── clients/{auth,profile,engine}.client.ts
│   │   ├── handlers/{orchestration,health}.handler.ts
│   │   └── helpers/auth.helper.ts
│   ├── job/, message/                     # pg-boss и Kafka, топики не заданы
│   └── proxy/
│       ├── router.ts                      # маршруты, 404 и 401
│       ├── routes/{auth,profile}.routes.ts
│       ├── controllers/{auth,profile}.controller.ts
│       ├── adapter/response.ts, utils/{jwt-from-proxy-request,parse-body,parse-query-params}.ts
│       └── middlewares/                   # не подключены
└── contracts/proto/                       # сгенерированный код (в .gitignore)
```

Маршруты: `POST /auth/anonymousSignIn`, `POST /auth/refreshTokens`, `POST /auth/verify`, `GET /auth/getMe`, `GET /profile/getMe`, `GET /profile/get`.
