# Structure

```
src/
├── app.ts
├── composition/build-application.ts     # wiring: job → message → health → process → grpc
├── application/
│   ├── application.ts                   # lifecycle: message → job → grpc
│   └── use-cases/
│       ├── health/{health,status,livez,readyz}.ts   # health-хендлеры
│       └── process/process.ts           # ЗАГЛУШКА: эхо + VALIDATION_FAILED на пустой текст
├── infrastructure/
│   ├── ai/                              # AiService, фабрика, OpenRouterClient — НЕ подключён
│   │                                    # (логика сценариями → src/infrastructure/ai/CONTEXT)
│   ├── app/                             # app.config
│   ├── db/                              # DbService + пустая schema.prisma (моделей нет),
│   │                                    # слой по решению не трогаем
│   ├── grpc/                            # service.ts: регистрация RemyService + Health
│   │   └── handlers/{remy,health,callback.error}
│   ├── job/, message/                   # продюсеры/консьюмеры пустые
│   └── utils/sleep.ts
└── contracts/proto/                     # сгенерированные ts-proto (gitignored)
```
