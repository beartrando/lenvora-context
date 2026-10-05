# Architectural rules

- Потребители: `gateway` (health-агрегатор, orchestration-клиент),
  `orchestration` (auth/profile/gusteau-клиенты), `streaming` (profile-клиент).
  Фактически не используется в `auth`, `profile` (заявлен в package.json, но не импортируется),
  `remy`, `gusteau`, `speech-to-text` (не заявлен).
- Фабрика клиента должна создаваться отдельно на каждый retry — ретрай спрятан внутри `call`.
- Контракт unary-only: `call()` рассчитан на одиночный ответ и ретрай, stream/bidi-RPC
  не поддерживаются. Для канала `streaming → speech-to-text` создаётся отдельный
  grpc-js клиент (`services/streaming/src/infrastructure/grpc/clients/speech-to-text.client.ts`)
  — без ретрая (реконнект потока = потеря сессии).
