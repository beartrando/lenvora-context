# Architectural rules

- Авторизация RPC в этом сервисе не настроена (`AuthService` не создаётся в composition).
- `createProduct` типизирует зависимость конкретным `PrismaProductRepository`
  (не доменным интерфейсом) — факт, зафиксирован как есть.
