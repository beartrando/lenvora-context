# Structure

```
src/
├── app.ts
├── composition/build-application.ts     # wiring: job → message → health → process → grpc
├── application/
│   ├── application.ts                   # lifecycle: message → job → grpc
│   └── use-cases/
│       ├── health/{health,status,livez,readyz}.ts   # восстановлены после «remove redundant code»
│       └── process/process.ts           # ЗАГЛУШКА: эхо + VALIDATION_FAILED на пустой текст
├── infrastructure/
│   ├── ai/                              # AiService, фабрика, OpenRouterClient — НЕ подключён
│   │                                    # (логика сценариями → src/infrastructure/ai/CONTEXT)
│   ├── app/                             # app.config
│   ├── db/                              # DbService + пустая schema.prisma (моделей нет),
│   │                                    # слой по решению не трогаем (seed удалён как мёртвый)
│   ├── grpc/                            # service.ts: регистрация RemyService + Health
│   │   └── handlers/{remy,health,callback.error}
│   ├── job/, message/                   # продюсеры/консьюмеры пустые
│   └── utils/sleep.ts
└── contracts/proto/                     # сгенерированные ts-proto (gitignored)
```

Удалено как мёртвый код: `domain/` (VO/сущности без единого use-case),
`application/ports/token-service.port.ts`, `infrastructure/auth/`,
`infrastructure/ai/configs/`, `infrastructure/message/config.ts` (дубль с дефолтами
`auth-client`), `infrastructure/message/interfaces/use-case.interface.ts`,
`infrastructure/job/configs/job.consumers.config.ts`, пустой `infrastructure/db/seed/`.
