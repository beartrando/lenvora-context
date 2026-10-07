# Entry points

- Точка входа кода: `services/profile/src/app.ts` → `composition/build-application.ts` → `Application.start()` (message → job → db → grpc).
- gRPC-сервер: `0.0.0.0:${GRPC_PORT}` (50051), регистрация `ProfileService` + `HealthService` в `infrastructure/grpc/service.ts`.
- Методы (proto/profile.proto): `GetOrCreate(common.empty.Empty) → ProfileResponse` (нужен metadata `authorization: Bearer …`), `Get(ProfileIdRequest) → ProfileResponse`, `GetByUser(auth.UserIdRequest) → ProfileResponse`; health: `Check/Status/Livez/Readyz`.
- Kafka: consumer `user.created` (`MESSAGE_CONSUMER_TOPIC_USER_CREATED`, groupId `profile-service-${topic}`), producer `profile.created` (`MESSAGE_PRODUCER_TOPIC_PROFILE_CREATED`).
- Docker: `EXPOSE 50051`, CMD `pnpm --filter profile dev` / `node dist/app.js`, HEALTHCHECK `nc -z localhost 50051`; predeploy — `pnpm --filter profile migrate:deploy`.
- Compose: сервис `profile` (env_file `services/profile/.env`), `depends_on: kafka, pgbouncer`, publish-портов нет.
- Скрипты package.json: `dev`, `stage`, `build`, `start`, `test`, `proto:generate`, `prisma:generate`, `migrate*`, `seed`.
