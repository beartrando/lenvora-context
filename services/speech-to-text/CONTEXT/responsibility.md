# Responsibility

- gRPC-сервер `Health` на `:50051` (бизнес-RPC не зарегистрированы:
  `registerSpeechToTextService()` пуст, `SpeechToTextHandler` — заглушка без методов).
- Структура сервиса: Prisma-модели `Product`/`NutritionCoefficient`/`FoodLog` (БД
  `speech-to-text`), домен и use-cases продуктовой модели — клоны gusteau.
