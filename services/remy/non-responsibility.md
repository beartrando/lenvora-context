# Non-responsibility

- Не хранит диалоги и состояние: Prisma-схема без моделей и миграций, кэша нет.
- Не обрабатывает голос и аудио — это [streaming](../streaming/purpose.md) и [speech-to-text](../speech-to-text/purpose.md).
- Не работает с данными о еде — это [gusteau](../gusteau/purpose.md).
- Не обслуживает HTTP: внешний доступ идёт через [gateway](../gateway/purpose.md) (или orchestration-прокси).
- Не выдаёт токены и не аутентифицирует вызовы — это [auth](../auth/purpose.md); `ACCESS_SECRET` используется только инфраструктурным auth-модулем сервиса.
- Не является Kafka-продюсером/consumer'ом: `producers.config.ts` и `consumers.config.ts` пусты, `addConsumer` не вызывается.
- Не владеет промптами и бизнес-правилами ответов — в текущем виде AI-узел делает только транспорт запроса.
