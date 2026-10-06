# Dependency direction

Как в родителе. AI-клиент (`infrastructure/ai/`) пока не участвует в импортах вне
`infrastructure/ai/`. Слой `domain/`, порт `application/ports/token-service.port.ts`
и `infrastructure/auth/` удалены как мёртвый код — не были достижимы из `app.ts`
и не создавались ниоткуда.
