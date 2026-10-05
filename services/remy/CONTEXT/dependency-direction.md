# Dependency direction

Как в родителе. AI-клиент пока не участвует в импортах вне `infrastructure/ai/`;
домен `user/profile/session/common` используется только `auth/service.ts` и
`application/ports/token-service.port.ts` (AuthService никуда не заведён).
