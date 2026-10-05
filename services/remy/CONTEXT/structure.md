# Structure

```
src/
├── app.ts
├── composition/build-application.ts     # wiring: job → message → health → process → grpc
├── application/
│   ├── application.ts                   # lifecycle: message → job → grpc
│   ├── ports/token-service.port.ts
│   └── use-cases/
│       ├── health/{health,status,livez,readyz}.ts   # восстановлены после «remove redundant code»
│       └── process/process.ts           # ЗАГЛУШКА: эхо + VALIDATION_FAILED на пустой текст
├── domain/                              # VO/сущности без use-cases: common, profile, session, user
├── infrastructure/
│   ├── ai/                              # OpenRouterClient, конфиги, интерфейсы — НЕ подключён
│   ├── app/, auth/                      # AuthService нигде не создаётся
│   ├── db/                              # DbService + пустая schema.prisma (моделей нет),
│   │                                    # seed.ts пуст; слой по решению не трогаем
│   ├── grpc/                            # service.ts: регистрация RemyService + Health
│   │   └── handlers/{remy,health,callback.error}
│   ├── job/, message/                   # продюсеры/консьюмеры пустые
│   └── utils/sleep.ts
└── contracts/proto/                     # сгенерированные ts-proto (gitignored)
```
