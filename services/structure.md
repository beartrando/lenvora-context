# Structure

```text
services/
├── <9 сервисов, см. children.md>
├── postgres/    # данные/конфиги postgres для docker-compose
└── pgbouncer/   # pgbouncer.ini, userlist.txt
```

Общий шаблон Node-сервиса (проверен по auth, profile, gateway, orchestration; остальные повторяют его):

```text
<service>/
├── package.json          # dev/stage/build/test, proto:generate, prisma-*, migrate*
├── Dockerfile            # base → deps → dev / build → prod / predeploy (миграции)
├── jest.config.js, tsconfig.json, tsconfig.test.json, nodemon.json
├── .env / .env.dist      # переменные окружения
└── src/
    ├── app.ts                    # единственная точка входа
    ├── composition/build-application.ts   # composition root
    ├── application/              # application.ts (lifecycle) + use-cases/ + ports/
    ├── domain/                   # сущности, value objects, интерфейсы репозиториев
    ├── infrastructure/           # app, auth(JWT), db(prisma), grpc, job(pg-boss), message(kafka), proxy
    ├── contracts/proto/          # сгенерированный код из proto/, в .gitignore
    └── utils/
```

- Prisma-схема: `src/infrastructure/db/prisma/schema.prisma` (+ `migrations/` у сервисов с миграциями).
- Kafka: `src/infrastructure/message/configs/{message,consumers,producers}.config.ts`.
- gRPC-порт: `GRPC_PORT` (по умолчанию 50051) в конфиге под `src/infrastructure/grpc/` — `config.ts` (auth, profile, orchestration), `config/config.ts` (gateway), `configs/grpc.config.ts` (streaming, gusteau, remy, speech-to-text).
- Порядок старта `Application.start()`: message → job (pg-boss + Kafka-bridge) → db → grpc; у gateway — grpc → http, у streaming — message → job → grpc → websocket, у gusteau/remy/speech-to-text шага db нет; остановка в обратном порядке, обработчики SIGINT/SIGTERM.
- gateway — вариант шаблона без domain/db: express-сервер + gRPC-сервер + gRPC-клиент orchestration.

Подключение сервисов к workspace и инфраструктуре — в корне ([корень](../purpose.md)); контракты — в [proto](../proto/purpose.md).
