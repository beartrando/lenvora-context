# Architectural rules

- Новый внешний путь → `infrastructure/proxy/routes/*` (регистрация в `Router`) +
  контроллер → use-case → gRPC-клиент.
- `SERVICE_AUTH_URL`, `SERVICE_PROFILE_URL`, `SERVICE_GUSTEAU_URL` — адреса клиентов.
- Kafka/pg-boss присутствуют только для health: продюсеров/консьюмеров нет.
