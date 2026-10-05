# Dependency direction

- `ai/` → только сам себя и SDK `openai`. Никаких импортов из `application/`,
  `contracts/`, `@shared/*` нет — модуль не знает о Remy.
- Наружу направлен абстракцией, а не реализацией: потребителю нужен
  `AiClient`/`AiService`, а `OpenRouterClient` и env-конфиг скрыты за фабрикой.
- Ожидаемое подключение (по TODO в use-case): `application/` → AI-порт → `AiService`,
  сборка в `composition/build-application.ts`. Порт в `application/ports/` пока не заведён.
