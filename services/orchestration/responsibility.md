# Responsibility

- gRPC-сервер `Orchestration` (`Proxy`) + `Health` на `:50051`.
- Собственный `Router` (метод + путь → handler) поверх `ProxyRequest`:
  маршруты `POST /auth/anonymousSignIn`, `GET /auth/getMe`, `POST /auth/refreshTokens`,
  `POST /auth/verify`, `GET /profile/getMe`, `GET /profile/get`.
- gRPC-клиенты `auth` и `profile` (через `@shared/grpc-client-manager`).
- Транспортная авторизация: middleware `jwt-from-proxy-request`, `authorizationRequired` у маршрута.
- Prisma-подключение с `check()` (схема без моделей — используется только health-проверка).
- Health-использование db/message/job.
