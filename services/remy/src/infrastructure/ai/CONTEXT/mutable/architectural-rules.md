# Architectural rules

- Модуль не содержит бизнес-логики Remy, промптов и знаний о gRPC: вход — текст,
  выход — строка. Всё, что связано с контрактом `Process`, живёт в use-case и хендлере.
- Новый провайдер добавляется только через `AiClient` + ветка в `createAiClient`;
  потребители не должны импортировать `clients/openrouter.ts` напрямую.
- Модуль не читает env: конфиг (`AiConfig`) передаётся в `AiService` явно,
  env-конфиг `configs/openrouter.ts` удалён как мёртвый код.
- Модуль не подключён: `src/application/use-cases/process/process.ts` — эхо-заглушка,
  TODO на `infrastructure/ai`. Подключение = порт в `application/` + wiring в
  `composition/build-application.ts` (по шаблону gusteau), плюс замена заглушки.
- Тесты — колокейтед, `src/infrastructure/ai/service.spec.ts` (мокается фабрика
  `createAiClient`, сетевых вызовов нет): `pnpm --filter remy test -- src/infrastructure/ai`.
- `tsc --noEmit` / `pnpm --filter remy build` — зелёные; подозрительно чистый вывод
  диагностики проверять осознанно (см. architectural-rules сервиса remy).
- `CONTEXT/` этого модуля хранится в репозитории контекста:
  `context/services/remy/src/infrastructure/ai/CONTEXT/` (подмодуль `context/`).
