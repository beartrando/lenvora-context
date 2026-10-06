# Dependency direction

Как в родителе. Специфика: `domain/` содержит только value-objects (сущностей и
интерфейсов репозиториев нет); use-cases `auth/*` и `profile/*` — тонкие обёртки над
gRPC-клиентами; контроллеры `infrastructure/proxy/controllers/*` импортируют use-cases.
