# Architectural rules

- Ответ прокси возвращается как `application/x-protobuf`; при заголовке `x-debug-json` — JSON.
- health-эндпоинты входят в `SERVER_ROUTES` (не проксируются).
- Мёртвый код: `src/healthcheck.js`; `request-logger.middleware.ts` не подключён.
