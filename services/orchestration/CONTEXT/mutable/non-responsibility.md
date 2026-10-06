# Non-responsibility

- Приём HTTP-трафика и конвертация HTTP ↔ gRPC (это gateway) — orchestration получает
  уже готовый `ProxyRequest`. Также не его зона: бизнес-логика профилей и
  аутентификации (→ profile / auth), транспортные слои shared (→ shared).
