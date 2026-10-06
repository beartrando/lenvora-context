# Dependency direction

kafka-manager → logger. Не зависит от сервисов и от proto. Потребители:
`services/*/src/infrastructure/message/service.ts` и pg-boss-manager.
