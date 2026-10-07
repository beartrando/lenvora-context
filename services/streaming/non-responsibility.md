# Non-responsibility

- Не реализует распознавание речи — только bidi-клиент [speech-to-text](../speech-to-text/purpose.md).
- Не выдаёт и не обновляет токены: выпуск и refresh — [auth](../auth/purpose.md); здесь только локальная проверка JWT и сверка профиля.
- Не хранит данные: Prisma-схема без моделей и миграций, состояние — только in-memory.
- Не обслуживает HTTP-API — этим занимается [gateway](../gateway/purpose.md).
- Не владеет доменом питания или ИИ: соответствующие use-case'ы и Kafka-consumer'ы к wiring не подключены.
- Не работает consumer'ом Kafka: `message/configs/consumers.config.ts` пуст, `addConsumer` нигде не вызывается.
