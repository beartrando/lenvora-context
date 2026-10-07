# Architectural rules

- Оркестратор не хранит состояние: Prisma-схема содержит только `datasource` (без моделей и миграций), клиент используется лишь для health-проверки `SELECT 1`.
- Роутинг — точное совпадение `method + path` (path-параметров нет); дубликат маршрута бросает ошибку при регистрации.
- Флаг `authorizationRequired` у маршрута сохраняется, но в `Router.handle()` не проверяется: токен извлекается и верифицируется внутри контроллеров (`jwtFromProxyRequest` → `AuthService.verifyAccessToken`).
- Публичные маршруты объявлены при регистрации: `anonymousSignIn`, `refreshTokens`, `verify` (`authorizationRequired = false`), но фактическая проверка всё равно лежит в контроллере.
- gRPC-клиенты — синглтоны на уровне модуля (`authClient`, `profileClient`), адреса берутся из `grpcConfig.serviceAuthUrl/…ProfileUrl/…GusteauUrl`.
- Каждый use-case — тонкая обёртка над клиентом; в `anonymousSignIn` контроллера «нет ответа» → `ServiceUnavailableError`, невалидный ответ → `InvalidResponseError`.
- Ошибки маршрута: 404 — `NotFoundError`, `TokenExpiredError` — 401 `TOKEN_EXPIRED`; прочие исключения уходят в `callbackError` gRPC-хендлера.
- Redis (`infrastructure/cache`) и `infrastructure/proxy/middlewares` существуют, но в `build-application.ts` не подключены.
