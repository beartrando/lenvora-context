# Responsibility

- gRPC-операции: `CreateProduct`, `GetProduct`, `GetNutritionCoefficient`, `UpdateNutritionCoefficient`, `CreateFoodLog`, `GetFoodLogs` — все ответы через `oneof output { объект | ErrorObject }`.
- Доменные правила расчёта и валидации продуктов/коэффициентов/дневника (value object'ы с ограничениями).
- Персистентность: Prisma-репозитории и бд-мапперы для `Product`, `NutritionCoefficient`, `FoodLog`; миграции и сид.
- Протокольные мапперы домен ↔ proto для gRPC-хендлера.
- Инфраструктурная доступность: health `check/status/livez/readyz` (pg-boss, Kafka), старт/останов сервиса.
- Готовность Kafka/pg-boss: конфиги тем есть, активных продюсеров и consumer'ов нет.
