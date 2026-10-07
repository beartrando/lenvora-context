# Responsibility

- gRPC-операция `Remy.Process`: принимает `RemyRequest{ text }`, возвращает `oneof output { text | ErrorObject }`; пустой текст → error code 4 (VALIDATION_FAILED).
- gRPC health `check/status/livez/readyz` с проверками pg-boss и Kafka.
- AI-подсистема (дочерний узел `src/infrastructure/ai`): фасад `AiService`, фабрика клиента, конфиг AI из env.
- Инфраструктурная инициализация: Kafka, pg-boss, старт/останов сервиса, конфиг приложения.
- Тесты: юнит-тесты use-case'а и AI-фасада, функциональный тест против живого gRPC-сервера.
