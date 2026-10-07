# remy/src/infrastructure/ai — AI-клиент сервиса remy: порт, провайдер, конфиг

Инкапсулирует вызов LLM для remy: порт `AiClient`, фабрика `createAiClient`, реализация OpenRouter через SDK `openai`, конфиг из env и фасад `AiService`.

Не знает о gRPC и use-case'ах remy; в `composition/build-application.ts` сервис пока не подключён.
