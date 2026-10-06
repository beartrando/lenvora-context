# Responsibility

- HTTP-сервер Express (`HTTP_PORT ?? 9090`): `/gateway/{health,full-health,status,livez,readyz}`,
  весь остальной трафик → `proxyController.proxy`.
- gRPC-сервер `Health` на `:50051`.
- `HealthAggregator` — health-фан-аут по всем сервисам из `SERVICE_*_URL`
  (remy, auth, profile, orchestration, gusteau, streaming).
- Конвертация HTTP-запроса в `ProxyRequest` и вызов gRPC `orchestration:50051`.
