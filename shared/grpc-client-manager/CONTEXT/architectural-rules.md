# Architectural rules

- Потребители: `gateway` (health-агрегатор, orchestration-клиент),
  `orchestration` (auth/profile/gusteau-клиенты), `streaming` (profile-клиент).
  Фактически не используется в `auth`, `profile` (заявлен в package.json, но не импортируется),
  `remy`, `gusteau`, `speech-to-text` (не заявлен).
- Фабрика клиента должна создаваться отдельно на каждый retry — ретрай спрятан внутри `call`.
